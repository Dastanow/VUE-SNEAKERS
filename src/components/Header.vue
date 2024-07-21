<script setup>
const emit = defineEmits(['openDraver'])
defineProps({
  totalPrice: Number
})
</script>

<template>
     <header class="flex justify-between border-b border-slate-300 px-10 py-8">
    <router-link to="/">
      <div  class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10">
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-500">Магазин лучших товаров</p>
        </div>
      </div>
    </router-link>

      <ul class="flex items-center gap-10">

        <li @click="() => emit('openDraver')" class="flex items-center gap-4 cursor-pointer text-gray-400 hover:text-black">
          <img src="/cart.svg" alt="">
          <span>{{totalPrice}} руб.</span>
        </li>
        <router-link to="favorites">
          <li class="flex items-center gap-4 cursor-pointer text-gray-400  hover:text-black">
          <img src="/heart.svg" alt="">
          <apan>Заклатки</apan>
        </li>
        </router-link>
        <li class="flex items-center gap-4 cursor-pointer text-gray-400  hover:text-black">
          <img src="/profile.svg" alt="">
          <span>Профиль</span>
        </li>
      </ul>
    </header>
</template>
