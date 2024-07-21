<script setup>
import Card from './Card.vue'
defineProps({
    items: Array,
    isFavorits: Boolean
})
const emit = defineEmits(['addFavorit', 'addToItems'])

</script>

<template>
    <div class="grid grid-cols-4 gap-10" v-auto-animate>
        <Card 
        v-for="item in items" :key="item.id"
         :id="item.id"
         :image-url="item.imageUrl"
         :isAdded="item.isAdded"
         :title="item.title"
         :prise="item.price"
         :onClickAdd="isFavorits ? null : () => emit('addToItems', item)"
         :isFavorit="item.isFavorit"
         :onClickFavorit="isFavorits ? null : () => emit('addFavorit', item)"
         />
        </div>
</template>