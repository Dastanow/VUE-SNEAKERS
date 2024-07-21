<script setup>
defineProps({
    title: String,
    imageUrl: String,
    description: String
})
</script>

<template>
   <div class="flex items-center text-center flex-col w-72 mx-auto">
    <img height="70" width="70" :src="imageUrl" alt="Info image">
    <h2 class="mt-4 text-2xl font-medium">{{title}}</h2>
    <p class="text-gray-400 mt-2">{{description}}</p>
   </div>
</template>
    