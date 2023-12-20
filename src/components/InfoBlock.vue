<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String,
  buttonCreated: Boolean,
})

</script>

<template>
  <div class="flex flex-col items-center text-center w-72 mx-auto">
    <img height="80" width="80" :src="imageUrl" alt="InfoImage">
    <h2 class="mt-4 text-2xl font-medium">{{title}}</h2>
    <p class="text-gray-400 mt-2">{{description}}</p>
    <router-link to="/">
    <button
      v-if="buttonCreated"
      class="mt-4 bg-blue-500 cursor-pointer w-72 active:bg-green-700 rounded-xl py-2 text-white transition hover:bg-blue-600"
    >
      Вернуться назад
    </button>
    </router-link>
  </div>
</template>
