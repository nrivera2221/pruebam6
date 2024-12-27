<script setup>

import PokeList from './components/PokeList.vue';

</script>

<template>
  <header>
    <img class="logo" src="../public/logo-pokemon.png" alt="">
    <h1>¿Quién es ese Pokémon?</h1>

  </header>
  <PokeList />
</template>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.logo {
  width: 300px;
}
</style>