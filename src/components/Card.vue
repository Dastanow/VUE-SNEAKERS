<script setup>

 defineProps({
        id: Number,
        title: String,
        imageUrl: String,
        prise: Number,
        isAdded: Boolean,
        isFavorit: Boolean,
        onClickAdd: Function,
        onClickFavorit: Function,
    })

</script>
<template>
 <div class=" relative bg-white border border-slate-200 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2">
        <img v-if="onClickFavorit" @click="onClickFavorit" :src="!isFavorit ? '/like-1.svg' : '/like-2.svg' " alt="" class="absolute top-3 left-8">
        <img :src="imageUrl" alt="sneakers">
        <p class="mt-5">{{ title }}</p>
        <div class="flex justify-between mt-5">
          <div class="flex flex-col">
            <span class="text-">Цена:</span>
            <b>{{prise}} руб.</b>
          </div>
          <img v-if="onClickAdd" @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="plus">
        </div>
      </div>
</template>