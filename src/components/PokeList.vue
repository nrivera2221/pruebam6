<template>
    <h3 style="text-align: center;">Pokemones descubiertos: <span style="color: goldenrod;">{{ discoveredCount }}</span>
    </h3>
    <div>
        <ul>
            <PokeItem v-for="(pokemon, index) in paginatedPokemones" :key="index" :pokemon="pokemon" :index="index"
                :correctNames="correctNames" @discover="addToCorrectNames" />
        </ul>
    </div>

    <!-- paginacion -->
    <PokePagination :currentPage="currentPage" :totalPages="totalPages" @changePage="changePage" />
</template>

<script setup>
import { onMounted, ref, computed } from 'vue';
import axios from 'axios';
import PokeItem from './PokeItem.vue';
import PokePagination from './PokePagination.vue';

const pokemones = ref([]);
const correctNames = ref(new Set());
const currentPage = ref(1);
const pokemonesPorPage = 20; // pokemones por pagina

onMounted(async () => {
    try {
        const requests = [];
        for (let i = 1; i <= 151; i++) {
            requests.push(axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`));
        }
        const responses = await Promise.all(requests);
        pokemones.value = responses.map(response => response.data);
    } catch (error) {
        console.error('Error al obtener los Pokémon:', error);
    }
});

// calculo con propiedad computada
const discoveredCount = computed(() => correctNames.value.size);

// paginacion computada
const paginatedPokemones = computed(() => {
    const startIndex = (currentPage.value - 1) * pokemonesPorPage;
    const endIndex = startIndex + pokemonesPorPage;
    return pokemones.value.slice(startIndex, endIndex);
});

const changePage = (newPage) => {
    currentPage.value = newPage;
};
// calculo con propiedad computada para el total de paginas
const totalPages = computed(() => Math.ceil(pokemones.value.length / pokemonesPorPage));


const addToCorrectNames = (name) => {
    correctNames.value.add(name);
};
</script>

<style scoped>
ul {
    list-style-type: none;
    padding: 0;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

li {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-bottom: 10px;
    padding: 32px;
}

img {
    width: 100px;
    height: 100px;
    padding: 8px;
}

.pagination {
    display: flex;
    justify-content: center;
    margin-top: 20px;
}

button {
    margin: 0 10px;
}
</style>