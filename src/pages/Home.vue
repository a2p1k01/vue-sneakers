<script setup>
import CardList from "@/components/CardList.vue";

import {ref, onMounted, watch, reactive, inject} from "vue";
import axios from "axios";

const items = ref([]);

const { cart, addToCart, openDrawer, removeFromCart } = inject('cartActions')

const onClickPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
};

const filters = reactive({
  sortBy: "title",
  searchQuery: "",
});

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
};

const onSearchQuery = (event) => {
  filters.searchQuery = event.target.value;
};

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id,
      };
      item.isFavorite = true;
      const { data } = await axios.post(
          `https://48a80f272ea1efcb.mokky.dev/favorites`,
          obj
      );
      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(
          `https://48a80f272ea1efcb.mokky.dev/favorites/${item.favoriteId}`
      );
      item.favoriteId = null;
    }
  } catch (e) {
    console.log(e);
  }
};

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
        `https://48a80f272ea1efcb.mokky.dev/favorites`
    );

    items.value = items.value.map((item) => {
      const favorite = favorites.find(
          (favorite) => favorite.item_id === item.id
      );

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      };
    });
  } catch (e) {
    console.log(e);
  }
};

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `${filters.searchQuery}`;
    }

    const { data } = await axios.get(
        `https://48a80f272ea1efcb.mokky.dev/items`,
        { params }
    );
    
    // const { data } = await axios.get(
    //     `http://127.0.0.1:8000/items`,
    //     { params }
    // );
    
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }));
  } catch (e) {
    console.log(e);
  }
};

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
});

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
});

watch(filters, fetchItems);
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
    <div class="flex gap-4">
      <select
          @change="onChangeSelect"
          class="bg-white h-10 ml-10 py-2 px-3 transition border rounded-md outline-none"
      >
        <option value="name">По названию</option>
        <option value="price">Сначала дешёвые</option>
        <option value="-price">Сначала дорогие</option>
      </select>
      <div class="relative">
        <img
            src="/search.svg"
            alt="search"
            class="absolute left-4 top-3"
        />
        <input
            @input="onSearchQuery"
            type="text"
            placeholder="Поиск..."
            class="border rounded-xl py-2 pl-11 pr-4 outline-none transition focus:border-gray-400"
        />
      </div>
    </div>
  </div>

  <div class="mt-10">
    <CardList
        :items="items"
        @add-to-favorite="addToFavorite"
        @add-to-cart="onClickPlus"
    />
  </div>
</template>
