<template>
    <div class="fixed fixed top-0 right-0 z-1">
        <TransitionGroup name="slide-down">
            <Toast
                v-for="message in toastMessages"
                :key="message.id"
                :toastMessage="message"
                @closeToast="removeToast(message.id)">
            </Toast>   
        </TransitionGroup>
    </div>
    <div class="m-5">
        <div class="flex flex-row flex-wrap lg:flex-nowrap gap-4">
            <div class="grow">
                <Product @productData="addToCart"></Product>
            </div>
            <div class="shrink-0 w-full lg:w-1/3">
                <Cart
                    :cartData="cartData"
                    :totalPrice="totalPrice"
                    @removeProduct="removeFormCart">
                </Cart>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, provide } from 'vue';
import Cart from '@/components/Cart.vue';
import Product from '@/components/Product.vue';
import Toast from '@/components/Toast.vue';

const props = defineProps(['cartData']);

//要props的購物車資料
const cartData = ref([]);

//接受emit傳來的資料
const addToCart = (item)=> {
    const boolean = cartData.value.some((someItem)=>{
        return someItem.title == item.title
    });
    if(boolean){
        const findItem = cartData.value.find((cartItem)=>{
            return cartItem.title == item.title;
        })
        findItem.amount += 1;
    }else{  
        cartData.value.push(item);
    }
}

//刪除資料
const removeFormCart = (product) => {
    cartData.value = cartData.value.filter((item) => {
        return item.id !== product.id
    })
    triggerToast({
        type: 'error',
        message: `${product.title} 已刪除`
    });
}

//自動計算總價
const totalPrice = computed(()=>{
    return cartData.value.reduce((sum,item)=>{
        return sum + item.price * item.amount;
    },0)
});

//通知訊息
const toastMessages = ref([]);
const triggerToast = (messageData) => {
    console.log("接收到",messageData)

    const id = Date.now();
    toastMessages.value.push({
        id: id,
        ...messageData
    })
    setTimeout(()=>{
        toastMessages.value = toastMessages.value.filter((msg)=>{
            return msg.id !== id 
        })
    },1500)
}
//手動刪除通知訊息
const removeToast = (id)=> {
    console.log("刪除訊息",id)
    toastMessages.value = toastMessages.value.filter(msg => {
        return msg.id !== id
    })
}
provide('triggerToast', triggerToast);
</script>
