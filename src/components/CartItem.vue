<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
});

const emit = defineEmits(["onClickRemove"]);
</script>

<template>
  <div
    class="flex items-center border border-slate-200 p-4 rounded-xl gap-4"
  >
    <img :src="imageUrl" alt="sneaker" class="w-16 h-16" />

    <div class="flex flex-1 flex-col">
      <p>{{ title }}</p>

      <div class="flex flex-1 justify-between mt-2">
        <b>{{ price }} руб.</b>
        <img
          @click="emit('onClickRemove')"
          class="opacity-50 hover:opacity-100 hover:-rotate-180 cursor-pointer transition"
          src="/close.svg"
          alt="close"
        />
      </div>
    </div>
  </div>
</template>
