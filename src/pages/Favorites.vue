<script setup>
import CardList from '@/components/CardList.vue';
import axios from 'axios';
import { ref, onMounted } from 'vue';
const favorite = ref([])

onMounted(async () => {
    const { data } = await axios.get('https://05518d3b7870fe22.mokky.dev/favorides?_relations=items')
    favorite.value = data.map(obj => obj.item)
    console.log(data);
})
</script>



<template>
        <h2 class="text-3xl font-bold mb-8 ">Мои закладки</h2>

 <CardList  :items="favorite" is-favorits/>
</template>