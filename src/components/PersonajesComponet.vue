<script setup lang="ts">
import axios from 'axios';
import { onMounted, ref } from 'vue';
import ButtonTop from '@/components/ButtonTop.vue';
import AOS from 'aos';
import 'aos/dist/aos.css';

AOS.init();
const page = ref(1);
const collection = ref<Personaje[]>([])



interface Location {
    name: string
}

interface Personaje {
    id: number;
    name: string;
    status: string;
    species: string;
    gender: string;
    image: string;
    location: Location
}

const fetchApi = async (url: string) => {
    try {
        const res = await axios.get(url);
        const data = res.data
        collection.value.push(...data.results)

        AOS.refresh();

    } catch (error) {
        console.log(error);

    }

}

const statusData: { [key: string]: string } = {
    Alive: 'bg-green-400',
    Dead: 'bg-red-600',
    unknown: 'bg-gray-300'
}


const loadPersonajes = () => {
    fetchApi(`https://rickandmortyapi.com/api/character/?page=${page.value}`);
}

const loadMorePersonajes = () => {
    page.value++
    fetchApi(`https://rickandmortyapi.com/api/character/?page=${page.value}`);
}
onMounted(() => {
    loadPersonajes();
})


</script>


<template>
    <section
        class=" inset-0 -z-10 h-full w-full items-center px-5 py-24 [background:radial-gradient(125%_125%_at_50%_10%,#000_40%,#63e_100%)]">

        <div class="flex flex-wrap justify-center pt-8 transition-all">

            <div data-aos-duration="1000" data-aos="fade-up" data-aos-offset="200"
                class="bg-transparent shadow-lg shadow-cyan-500/50 rounded-xl m-4 p-6 flex flex-col"
                v-for="personaje in collection" :key="personaje.id">

                <img class="rounded-full my-1 w-3/4 self-center " :src="personaje.image" :alt="personaje.name">
                <h1 class=" text-white text-lg  font-bold">{{ personaje.name }}</h1>
                <div class="flex items-center gap-1">
                    <div :class="statusData[personaje.status]" class="rounded-full h-3 w-3"></div>
                    <h4 class="font-medium text-white">{{ personaje.status }} - {{ personaje.species }}</h4>
                </div>
                <h4 class="text-white">{{ personaje.location.name }}</h4>
            </div>
        </div>

        <div class="flex justify-center p-10">
            <button
                class=" bg-black text-white text-lg px-4 py-2 rounded hover:bg-blue-500 hover:text-white focus:outline-none focus:ring-2 focus:ring-blue-500"
                @click="loadMorePersonajes()">Mas personajes...</button>

            <ButtonTop />
        </div>

    </section>

</template>