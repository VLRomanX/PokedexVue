<template>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="" />
  <link
    href="https://fonts.googleapis.com/css2?family=Nunito&display=swap"
    rel="stylesheet"
  />
  <NavBar />
  <Pokedex />
</template>

<script setup lang="ts">
import NavBar from "./components/NavBar.vue";
import Pokedex from "./components/Pokedex.vue";
</script>

<style>
* {
  margin: 0px;
}

#app {
  margin: 0;
}
</style>

