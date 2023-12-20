<script setup>
import {onMounted, ref} from "vue";

import CardList from "@/components/CardList.vue";
import axios from "axios";
import InfoBlock from "@/components/InfoBlock.vue";

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get('https://48a80f272ea1efcb.mokky.dev/favorites?_relations=items')
    console.log(data);
    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err);
  }
})

</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
  <div v-if="favorites.length > 0">
    <CardList :items="favorites" is-favorites/>
  </div>
  <div  v-else>
    <InfoBlock
        title="Закладок нет :("
        description="Вы ничего не добавляли в закладки"
        image-url="/emoji-1.png"
        :button-created="true"
    />
  </div>
</template>
