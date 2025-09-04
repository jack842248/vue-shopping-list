<template>
    <div class="w-[360px] flex justify-between items-center fixed top-0 right-0 text-white bg-emerald-700 rounded shadow-xl p-3 m-3"
        :class="toastMessage.type === 'success'?'bg-emerald-700':'bg-red-700'">
        <p class="text-sm">{{ toastMessage.message }}</p>
        <button
            type="button"
            class="text-white/50 hover:cursor-pointer"
            @click="closeToast">
            <font-awesome-icon icon="fa-solid fa-xmark" />
        </button>
    </div>
</template>

<script setup>
const emit = defineEmits(['closeToast']);

defineProps({
    toastMessage: {
        type: Object,
        required: true
    }
});

const closeToast = ()=> {
    emit('closeToast',false)
}
</script>