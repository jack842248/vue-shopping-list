<template>
    <h2 class="text-3xl font-medium mb-3">商品列表</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-4">
        <template v-for="item in productData" :key="item">
            <div class="card border-1 border-neutral-200">
                <div class="card-img-top overflow-hidden">
                    <img class="transition-transform duration-500 aspect-16/9 object-cover" :src="'images/'+item.photo" alt="商品"/>
                </div>
                <div class="bg-white p-3">
                    <div class="text-xl mb-1">{{ item.title }}</div>
                    <div class="text-neutral-500 mb-4">{{ item.description }}</div>
                    <div class="text-sky-700 font-bold mb-5">${{ item.price.toLocaleString() }}</div>
                    <button
                        type="button"
                        class="bg-emerald-600 text-white w-full py-2 rounded hover:cursor-pointer hover:bg-emerald-600/80"
                        @click="sendData(item)">加入購物車
                    </button>
                </div>
            </div>
        </template>
    </div>
</template>

<script setup>
import { ref, inject } from 'vue';

const emit = defineEmits(['productData']);

//所有產品資料
const productData = ref([
    {
        title: '耳罩式藍牙耳機',
        description: '舒適配戴，支援降噪技術',
        price: 2490,
        photo: 'img-product1.jpg',
        amount: 1
    },
    {
        title: '耳罩式彩虹耳機',
        description: '舒適配戴，支援降噪技術',
        price: 1380,
        photo: 'img-product2.jpg',
        amount: 1
    },
    {
        title: '時尚藍牙耳機',
        description: '舒適配戴，支援降噪技術',
        price: 7990,
        photo: 'img-product3.jpg',
        amount: 1
    },
    {
        title: '機械式鍵盤',
        description: '時尚粉色系，打字手感極佳',
        price: 1890,
        photo: 'img-product4.jpg',
        amount: 1
    },
    {
        title: '無線滑鼠',
        description: '靜音按鍵設計，長效電池',
        price: 890,
        photo: 'img-product5.jpg',
        amount: 1
    }
])

//點擊“加入購物車”
const triggerToast = inject('triggerToast');
const sendData = (item)=> {
    //產生id
    const generateId = () => {
        const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
        let id = '-';
        for (let i = 0; i < 19; i++) {
            id += chars.charAt(Math.floor(Math.random() * chars.length));
        }
        return id;
    };
    emit('productData',{
        id: generateId(),
        title: item.title,
        description: item.description,
        price: item.price,
        photo: item.photo,
        amount: item.amount
    })
    triggerToast({
        type: 'success',
        message: `${item.title} 加入購物車`
    });
}
</script>
