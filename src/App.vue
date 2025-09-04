<template>
    <Transition name="slide-down">
        <Toast
            v-if="showToast"
            :toastMessage="toastMessage"
            @closeToast="showToast = false">
        </Toast>
    </Transition>
    <div class="m-5">
        <div class="flex flex-row flex-wrap lg:flex-nowrap gap-4">
            <div class="grow">
                <Product @productData="saveData"></Product>
            </div>
            <div class="shrink-0 w-full lg:w-1/3">
                <Cart
                    :cartData="cartData"
                    :totalPrice="totalPrice"
                    @removeProduct="removeData">
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
const saveData = (item)=> {
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
const removeData = (product) => {
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
const showToast = ref(false);
const toastMessage = ref({});
let toastTimeoutId = null;
const triggerToast = (messgaeData)=> {
    if (toastTimeoutId) {
        clearTimeout(toastTimeoutId);
    }
    toastMessage.value = messgaeData;
    showToast.value = true;
    toastTimeoutId = setTimeout(()=>{
        showToast.value = false;
        toastTimeoutId = null;
    },1500)
}
provide('triggerToast', triggerToast);
</script>
