<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
    pokemon: Object,
    index: Number,
    correctNames: Set,
});

const emit = defineEmits(['discover']);
const userInput = ref('');

const isDiscovered = computed(() => props.correctNames.has(props.pokemon.name));

const checkPokemonName = () => {
    const correctName = props.pokemon.name.toLowerCase();
    if (userInput.value.toLowerCase() === correctName) {
        emit('discover', props.pokemon.name);
        userInput.value = '';
    } else {
        alert('Nombre incorrecto, intenta de nuevo.');
    }
};
</script>

<template>
    <li>
        <img :src="pokemon.sprites.other.home.front_default" :style="{
            filter: isDiscovered ? 'none' : 'blur(5px) grayscale(100%)'
        }" alt="imagenPokemon" />
        <h3 v-if="isDiscovered">{{ pokemon.name }}</h3>
        <input v-else type="text" @keypress.enter="checkPokemonName" v-model="userInput" />
        <button @click="checkPokemonName" v-show="!isDiscovered">Descubrir</button>
    </li>
</template>



<style scoped>
img {
    width: 200px;
    height: 200px;
    padding: 12px;
}
</style>