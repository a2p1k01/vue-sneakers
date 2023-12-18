<script setup>
import { ref, onMounted, watch, reactive, provide, computed } from "vue";
import axios from "axios";

import Header from "@/components/Header.vue";
import CardList from "@/components/CardList.vue";
import Drawer from "./components/Drawer.vue";

const items = ref([]);

const cartPrice = computed(() =>
  cart.value.reduce((acc, item) => acc + item.price, 0)
);

const cart = ref([]);

const drawerOpen = ref(false);

const addToCart = (item) => {
  cart.value.push(item);
  item.isAdded = true;
};

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1);
  item.isAdded = false;
};

const onClickPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
};

const closeDrawer = () => {
  drawerOpen.value = false;
};

const openDrawer = () => {
  drawerOpen.value = true;
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
        parentId: item.id,
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
        (favorite) => favorite.parentId === item.id
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
      params.title = `*${filters.searchQuery}*`;
    }

    const { data } = await axios.get(
      `https://48a80f272ea1efcb.mokky.dev/items`,
      { params }
    );

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
  await fetchItems();
  await fetchFavorites();
});

watch(filters, fetchItems);

provide("cartActions", {
  cart,
  addToCart,
  removeFromCart,
  closeDrawer,
  openDrawer,
});
</script>

<template>
  <Drawer v-if="drawerOpen" :cart-price="cartPrice"/>
  <div class="mt-14 bg-white w-4/5 m-auto rounded-xl shadow-2xl">
    <Header :cost="0" @open-drawer="openDrawer" :cart-price="cartPrice" />
    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>
        <div class="gap-4">
          <select
            @change="onChangeSelect"
            class="bg-white h-10 ml-10 py-2 px-3 transition border rounded-md outline-none"
          >
            <option value="name">По названию</option>
            <option value="price">Сначала дешёвые</option>
            <option value="-price">Сначала дорогие</option>
          </select>
          <dir class="relative">
            <img
              src="/search.svg"
              alt="search"
              class="absolute left-14 top-3"
            />
            <input
              @input="onSearchQuery"
              type="text"
              placeholder="Поиск..."
              class="border rounded-xl py-2 pl-11 pr-4 outline-none transition focus:border-gray-400"
            />
          </dir>
        </div>
      </div>

      <div class="mt-10">
        <CardList
          :items="items"
          @add-to-favorite="addToFavorite"
          @add-to-cart="onClickPlus"
        />
      </div>
    </div>
  </div>
</template>

<style></style>
