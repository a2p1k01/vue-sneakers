<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
defineProps({
  cartPrice: Number,
})

const emit = defineEmits('openDrawer')

</script>

<template>
  <header class="flex justify-between border-b border-slate-100 px-10 py-8">
      <div class="flex items-center gap-3">
        <img src="/logo.png" alt="Logo" class="w-10">
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-500">Магазин лучших кроссовок</p>
        </div>
      </div>

      <ul class="flex items-center gap-10">
        <li @click="() => emit('openDrawer')" class="flex items-center cursor-pointer gap-2 text-gray-700 hover:text-black">
          <img src="/cart.svg" alt="cart">
          <b>{{ cartPrice }} руб.</b>
        </li>
        <li class="flex items-center cursor-pointer gap-2 text-gray-500 hover:text-black">
          <img src="/heart.svg" alt="cart">
          <p>Закладки</p>
        </li>
        <li class="flex items-center cursor-pointer gap-2 text-gray-500 hover:text-black">
          <img src="/profile.svg" alt="cart">
          <p>Профиль</p>
        </li>
      </ul>

    </header>
</template>