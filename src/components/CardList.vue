<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
  import Card from './Card.vue'

  defineProps({
    items: Array,
  })

  const emit = defineEmits(['addToFavorite', 'addToCart'])

</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card 
      v-for="item in items"
      v-bind:key="item.id"
      :id="item.id"
      :title="item.title"
      :image-url="item.imageUrl" 
      :price="item.price"
      :on-click-favorite="() => emit('addToFavorite', item)"
      :on-click-add="() => emit('addToCart', item)"
      :is-favorite="item.isFavorite"
      :is-added="item.isAdded"
    />
  </div>
</template>