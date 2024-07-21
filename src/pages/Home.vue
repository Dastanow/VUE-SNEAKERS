<script setup>
import {inject, onMounted, reactive, ref, watch} from 'vue'
import CardList from '../components/CardList.vue'
import axios from 'axios';
import debounce from 'lodash.debounce';

const items = ref([])


const {cart, addToItems,reamovFromCart} = inject('cart')

const filter = reactive({
  sortBy: '',
  searchQueri: ''
})

const changeSelct = (event) => {
  filter.sortBy = event.target.value
}
const onChangeItem = debounce((event) => {
  filter.searchQueri = event.target.value
}, 500)
const addFavorit = async (item) => {
  try {
    if (!item.isFavorit) {
      const obj = {
        item_id: item.id,
      }
      const { data } = await axios.post('https://05518d3b7870fe22.mokky.dev/favorides', obj)
      item.isFavorit = true;
      item.favoriteId = data.id
      console.log(data);
    } else {
      await axios.delete(`https://05518d3b7870fe22.mokky.dev/favorides/${item.favoriteId}`)
      item.isFavorit = false;
      item.favoriteId = null
    }

  } catch (err) {
    console.log(err)
  }
}
const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToItems(item)
  } else {
    console.log(item)
    reamovFromCart(item);
  }
  console.log(item)
  console.log(cart);
}
const fetchItems = async () => {
  try {
    const params = {
      sortBy: filter.sortBy
    }
    if (filter.searchQueri) {
      params.title = `*${filter.searchQueri}*`
    }
    const { data } = await axios.get('https://05518d3b7870fe22.mokky.dev/items', {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorit: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorits = async () => {
  try {
    const { data: favorites } = await axios.get('https://05518d3b7870fe22.mokky.dev/favorides')
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.item_id === item.id);

      if (!favorite) {
        return item;
      }
      return {
        ...item,
        isFavorit: true,
        favoriteId: favorite.id
      }
    })
    console.log(items.value);
  } catch (err) {
    console.log(err)
  }
}


onMounted(async () => {
  const localStor = localStorage.getItem('cart')
  cart.value = localStor ? JSON.parse(localStor) : [];

  
  await fetchItems();
  await fetchFavorits()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
}),
watch(filter, fetchItems)

watch(cart, () => {
    items.value =  items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
    })


</script>

<template>
     <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8 ">Все кросовки</h2>

        <div class="flex gap-4">
          <select @change="changeSelct" class="py-2 px3 border rounded-md outline-none">
            <option value="name">Все</option>
            <option value="price">По цене(диш.)</option>
            <option value="-price">По цене(дор.)</option>
          </select>
          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="">
            <input @input="onChangeItem" class="border rounded-mb py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              placeholder="Поиск..." type="text">
          </div>
        </div>
      </div>
      <div class="mt-10">
        <CardList :items="items" @addFavorit="addFavorit" @addToItems="onClickAddPlus" />
      </div>
</template>