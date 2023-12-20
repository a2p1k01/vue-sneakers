<script setup>
import Header from "@/components/Header.vue";
import Drawer from "./components/Drawer.vue";

import {computed, provide, ref, watch} from "vue";
import axios from "axios";

const isCreatedOrder = ref(false);
const cartButtonDisabled = computed(() => isCreatedOrder.value || cart.value.length === 0)
const drawerOpen = ref(false);
const cartId = ref(null);

const cartPrice = computed(() =>
    cart.value.reduce((acc, item) => acc + item.price, 0)
)

const openDrawer = () => {
  drawerOpen.value = true;
};

const cart = ref([]);

const createOrder = async () => {
  try {
    isCreatedOrder.value = true;
    const { data } = await axios.post(`https://48a80f272ea1efcb.mokky.dev/orders`, {
      items: cart.value,
      cartPrice: cartPrice.value,
    })
    cart.value = [];
    cartId.value = data.id;
    return data;
  } catch (err) {
    console.log(err);
  } finally {
    isCreatedOrder.value = false;
  }
}

const closeDrawer = () => {
  drawerOpen.value = false;
};

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1);
  item.isAdded = false;
};

const addToCart = (item) => {
  cart.value.push(item);
  item.isAdded = true;
};

watch(cart, () => {
  localStorage.setItem('cart', JSON.stringify(cart.value));
}, {deep: true});

provide("cartActions", {
  cart,
  addToCart,
  removeFromCart,
  closeDrawer,
  openDrawer,
});

</script>

<template>
  <Drawer
      v-if="drawerOpen"
      :cart-price="cartPrice"
      @create-order="createOrder"
      :is-created-order="isCreatedOrder"
      :cart-id="cartId"
      :cart-button-disabled="cartButtonDisabled"
  />
  <div class="mt-14 bg-white w-4/5 m-auto rounded-xl shadow-2xl">
    <Header :cost="0" @open-drawer="openDrawer" :cart-price="cartPrice" />
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style></style>
