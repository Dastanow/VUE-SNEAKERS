<script setup>
import DraverHead from './DraverHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'
import { computed, inject, ref } from 'vue';
import axios from 'axios';
const props = defineProps({
    totalPrice: Number,
    vatPrice: Number,
})
const isCreating = ref(false)
const orderId = ref(null)
const {cart, openDraver} = inject('cart')

const cartEmpty = computed(() => cart.value.length === 0)

const DisabledButton = computed(() => isCreating.value || cartEmpty.value)

const ceateOrder = async () => {
  try {
    isCreating.value = true
    const {data} = await axios.post(`https://05518d3b7870fe22.mokky.dev/order`, {
        items: cart.value,
      totalPrice: props.totalPrice.value,
    }
)


cart.value = []
orderId.value = data.id
  }
  catch (err) {
    console.log(err);
  }
  finally {
    isCreating.value = false
  }
} 
</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DraverHead/>

        <div v-if="!totalPrice || orderId" class="flex h-full items-center" >
            <InfoBlock v-if="!totalPrice && !orderId"
             title="Корзина пустая" 
            description="Добавьте хотя бы одну пару кроссовок, чтоьбы сделать заказю"
            imageUrl="/package-icon.png"/>

            <InfoBlock v-if="orderId"
             title="Заказ оформлен" 
            description="Ваш заказ №18 скоро будет передан курьерской доставке"
            imageUrl="/order-success-icon.png"/>

        </div>
         <div v-else>
             <CartItemList/>

          <div class="flex flex-col gap-4 mt-7">
          <div class="flex gap-2">
         <span>Итого:</span>
           <div class="flex-1 border-b border-dashed" ></div>
          <b>{{ totalPrice }} Р</b>
          </div>
          <div class="flex gap-2">
              <span>Налог 5%</span>
              <div class="flex-1 border-b border-dashed" ></div>
              <b>{{ vatPrice }} Р</b>
             </div>
             <button :disabled="DisabledButton" @click="ceateOrder" class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white       disabled:bg-slate-300  hover:bg-lime-600 active:bg-lime-700">Оформить заказ</button>
              </div>
     </div>
    </div>
</template>