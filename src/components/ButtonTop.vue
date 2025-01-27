<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';

const showButton = ref(false);


const handleScroll: () => void = () => {
    if (window.scrollY > 200) {
        showButton.value = true;
    } else {
        showButton.value = false;
    }
};

const scrollToTop: () => void = () => {
    window.scrollTo({ top: 0, behavior: "smooth" });
};

onMounted(() => {
    window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
    window.removeEventListener("scroll", handleScroll);
});
</script>

<template>
    <button v-show="showButton" @click="scrollToTop"
        class="fixed right-0 bottom-8 mr-[10%] border animate-bounce text-white p-2 rounded-full  hover:scale-110 active:scale-95 hover:bg-white hover:text-black">
        <i class="fa-solid fa-arrow-up text-2xl"></i>
    </button>


</template>