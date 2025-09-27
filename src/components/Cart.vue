<template>
    <h2 class="text-3xl font-medium mb-3">購物車</h2>
    <template v-if="getCartData.length">
        <ul class="bg-white">
            <li v-for="item in getCartData" :key="item" class="flex justify-between items-center border-b-1 border-neutral-200 p-2 px-3">
                <div class="left">
                    <p>{{ item.title }}</p>
                    <p class="text-neutral-500">數量：{{ item.amount }}</p>
                </div>
                <div class="flex items-center">
                    <p class="text-neutral-500 mr-2">${{ (item.price * item.amount).toLocaleString() }}</p>
                    <button
                        type="button"
                        class="text-red-700 border border-red-700 px-3 py-1 rounded hover:cursor-pointer hover:bg-red-700 hover:text-white"
                        @click="removeProduct(item)">移除
                    </button>
                </div>
            </li>
        </ul>
        <p class="font-bold text-right mt-3 mr-2">總金額：${{ totalPrice.toLocaleString() }}</p>
    </template>
    <p v-else class="text-neutral-500">目前無資料</p>
</template>

<script setup>
import { computed } from 'vue'; 

const emit = defineEmits(['removeProduct'])

const props = defineProps({
    cartData: {
        type: Object,
        required: true
    },
    totalPrice: {
        type: Number,
        required: true
    }
})

//接收app傳來的資料
const getCartData = computed(()=>{
    return props.cartData;
})

//點擊移除
const removeProduct = (product)=> {
    emit("removeProduct", product);
}
</script>