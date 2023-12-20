<script setup>
import {onMounted, ref} from "vue";

import CardList from "@/components/CardList.vue";
import axios from "axios";
import InfoBlock from "@/components/InfoBlock.vue";

const orders = ref([]);

// onMounted(async () => {
//   try {
//     const { data } = await axios.get('https://48a80f272ea1efcb.mokky.dev/orders?_relations=items')
//     orders.value = data.map((obj) => ({
//       ...obj.items,
//     }))
//     console.log(orders.value)
//   } catch (err) {
//     console.log(err);
//   }
// })

</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои покупки</h2>
  <h2 class="flex flex-col items-center text-center text-gray-400 m-8">Страница в разработке...</h2>
  <div v-if="orders.length > 0">
    <CardList :items="orders"/>
  </div>
  <div v-else>
    <InfoBlock
        title="Покупок нет :("
        description="Вы ещё ничего не покупали..."
        image-url="/emoji-2.png"
        :button-created="true"
    />
  </div>
</template>
