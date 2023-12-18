<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cart, removeFromCart } = inject('cartActions')

const onClickRemove = () => {
  addToCart(item);
}

</script>

<template>
  <div class="flex flex-col gap-5">
    <CartItem
      v-for="item in cart"
      v-bind:key="item.id"
      :title="item.title"
      :image-url="item.imageUrl" 
      :price="item.price"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>