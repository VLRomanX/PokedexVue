<template>
  <div class="nav-container">
    <div class="search-container">
      <div>
        <input
          v-model="search_pokemon"
          type="text"
          placeholder="Pesquise um pokemon"
        />
      </div>
      <div>
        <button @click="findPokemon(search_pokemon)">Buscar</button>
      </div>
    </div>
    <div
      class="pokemon-container"
      v-if="
        pokemon &&
        pokemon.sprites &&
        pokemon.sprites.versions['generation-v']['black-white'].animated
          .front_default
      "
    >
      <div>ID: {{ pokemon.id }}</div>
      <div>Nome: {{ pokemon.name }}</div>
      <img
        class="pokemon-sprite"
        :src="
          pokemon.sprites.versions['generation-v']['black-white'].animated
            .front_default
        "
        alt="Pokemon image"
      />
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

interface PokemonData {
  id: number;
  name: string;
  sprites: {
    versions: {
      "generation-v": {
        "black-white": {
          animated: {
            front_default: string;
          };
        };
      };
    };
  };
}

const search_pokemon = ref("");
const pokemon = ref<PokemonData | null>(null);

async function findPokemon(pokemonName: string) {
  const response = await axios.get(
    `http://localhost:3000/pokemon/${pokemonName}`
  );
  pokemon.value = response.data;
  console.log(pokemon);
}
</script>
<style scoped>
.search-container {
  display: flex;
  flex-direction: row;
}
.pokemon-container {
  display: flex;
  flex-direction: column;
}
.pokemon-sprite {
  width: 100px;
}
</style>
