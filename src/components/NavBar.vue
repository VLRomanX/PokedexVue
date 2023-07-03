<template>
  <nav class="nav-container">
    <div class="img-container">
      <img class="pokeapi_img" :src="logo_img" alt="pokeapi_img" />
    </div>
    <div class="search-container">
      <SearchBar />
    </div>
  </nav>
</template>
<script setup lang="ts">
import SearchBar from "./SearchBar.vue";
import { ref } from "vue";

const logo_img = ref(
  "https://raw.githubusercontent.com/PokeAPI/media/master/logo/pokeapi_256.png"
);
</script>
<style scoped>
.nav-container {
  background-color: #ef5350;
}
</style>
