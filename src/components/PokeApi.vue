<template>
  <h2 v-if="pokemons.name"> {{ pokemons.name }}</h2>
  <img v-if="pokemons.sprites" :src="pokemons.sprites.other.home.front_default" alt="Pokemon Picture" />
</template>
<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';

const pokemons = ref([])
onMounted(async () => {
  try {
      const response = await axios.get('https://pokeapi.co/api/v2/pokemon/25');
      pokemons.value = response.data;

  } catch (error) {
      console.error('Error al obtener los usuarios:', error);
  }
});
</script>