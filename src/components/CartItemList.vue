<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const {cart, reamovFromCart} = inject('cart')
</script>

<template>
    <div class="flex flex-col gap-5 flex-1 justify-between" v-auto-animate>
        <CartItem v-for="item in cart" 
        :key="item.id"
        :title="item.title"
         :price="item.price" 
         :imageUrl="item.imageUrl"
         @on-click-remuve="() => reamovFromCart(item)"
          />       
    </div>
</template>