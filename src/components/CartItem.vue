<script setup>
const emit = defineEmits(['onClickRemuve'])
defineProps({
    title: String,
    price: Number,
    imageUrl: String
})
</script>


<template>
    <div class="flex border border-slate-100 p-4 rounded-xl gap-4">
        <img class="w-16 h-16" :src="imageUrl" alt="Sneacers">

        <div class="flex flex-col flex-1">
            <p>{{ title }}</p>
            <div class="flex items-center gap-20 mt-2">
                <b class="flex-1">{{ price }}</b>
                <img @click="emit('onClickRemuve')" class="opacity-40 hover:opacity-100 cursor-pointer" src="/close.svg" alt="close">
            </div>
        </div>

    </div>
</template>