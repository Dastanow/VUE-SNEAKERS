<script setup>
import { computed, provide, ref, watch } from 'vue';


import Header from './components/Header.vue'
import Draver from './components/Draver.vue'
import { RouterView } from 'vue-router';

const cart = ref([])
const clickDraver = ref(false)

const openDraver = () => {
  clickDraver.value = true
}
const addToItems = (item) => {
  cart.value.push(item)
  item.isAdded = true
}
const reamovFromCart = (item) => {
  console.log(item)
  cart.value.filter(i => i.id !== item.id)
  item.isAdded = false 
  cart.value = cart.value.filter(i => i.id !== item.id)
}

const closeDraver = () => {
  clickDraver.value = false
}

const totalPrice = computed(
  () => cart.value.reduce((acc, item) => acc + item.price, 0)
)
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))



  watch(
    cart,
    () => {
      localStorage.setItem('cart', JSON.stringify(cart.value))
    },
    {deep: true}
  )
provide('cart', {
  cart,
  openDraver,
  closeDraver,
  reamovFromCart,
  addToItems
})
</script>

<template>
  <Draver v-if="clickDraver" :vatPrice="vatPrice" :totalPrice="totalPrice"/>
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-10">
    <Header :total-price="totalPrice" @open-draver="openDraver" />
    <div class="p-10">
      <RouterView />
    </div>
  </div>
</template>

<style></style>