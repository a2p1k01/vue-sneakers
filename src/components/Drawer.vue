<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
  import DrawerHead from './DrawerHead.vue';
  import CartListItem from './CartListItem.vue';
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <CartListItem />

    <div class="flex flex-col gap-5 mt-7">
      <div class="flex gap-2">
        <span>Итого: </span>
        <div class="flex-1 border-b"></div>
        <b>12900 руб.</b>
      </div>
      <div class="flex gap-2">
        <span>Доставка 5%: </span>
        <div class="flex-1 border-b"></div>
        <b>900 руб.</b>
      </div>
      <button class="mt-4 bg-green-500 cursor-pointer disabled:bg-slate-400 w-full active:bg-green-700 rounded-2xl py-2 text-white transition hover:bg-green-600">
        Оформить заказ
      </button>
    </div>
  </div>
</template>
