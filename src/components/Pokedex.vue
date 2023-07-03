<template>
  <div class="pokemon-list">
    <div
      class="pokemon-item"
      :style="{ backgroundColor: getBackgroundColor(pokemon) }"
      v-for="pokemon in pokemons"
      :key="pokemon.name"
    >
      <div class="pokemon-img">
        <img
          class="pokemon-sprite"
          :src="
            pokemon.sprites.versions['generation-v']['black-white'].animated
              .front_default
          "
          alt="Pokemon image"
        />
      </div>
      <div class="pokemon-name" :style="{ color: getBackgroundColor(pokemon) }">
        <strong>{{ pokemon.name }}</strong>
      </div>
      <div class="pokemon-types">
        <div
          class="pokemon-type"
          v-for="poke in pokemon.types"
          :key="poke.type.name"
          :style="{ color: getBackgroundColor(pokemon) }"
        >
          {{ poke.type.name }}
        </div>
      </div>
      <div class="pokemon-id">
        <strong>ID: {{ pokemon.id }}</strong>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";

interface PokemonData {
  id: number;
  name: string;
  types: { type: { name: string } }[];
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

const pokemons = ref<PokemonData[]>([]);

async function findPokemons() {
  const response = await axios.get(
    `https://pokeapi.co/api/v2/pokemon?limit=18`
  );
  const data = await Promise.all(
    response.data.results.map((pokemon: any) =>
      axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemon.name}`)
    )
  );
  pokemons.value = data.map((res) => res.data);
}

const typeColors = {
  normal: "#A8A77A",
  fire: "#EE8130",
  water: "#6390F0",
  electric: "#F7D02C",
  grass: "#7AC74C",
  ice: "#96D9D6",
  fighting: "#C22E28",
  poison: "#A33EA1",
  ground: "#E2BF65",
  flying: "#A98FF3",
  psychic: "#F95587",
  bug: "#A6B91A",
  rock: "#B6A136",
  ghost: "#735797",
  dragon: "#6F35FC",
  dark: "#705746",
  steel: "#B7B7CE",
  fairy: "#D685AD",
};

function getBackgroundColor(pokemon: PokemonData) {
  const type = pokemon.types[0].type.name;
  return (typeColors as { [key: string]: string })[type] || "transparent";
}

onMounted(() => {
  findPokemons();
});
</script>
<style scoped>
.pokemon-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.pokemon-item {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  width: 150px;
  height: 250px;
  margin: 10px;
  background-color: #feca1b;
  border-radius: 20px;
}

.pokemon-img {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  border-radius: 100px;
  width: 120px;
  height: 120px;
  overflow: hidden;
}

.pokemon-img::after {
  content: "";
  background-color: white;
  opacity: 0.7;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  position: absolute;
}

.pokemon-sprite {
  width: 100px;
  height: 100px;
  z-index: 10;
  position: relative;
}

.pokemon-name {
  background-color: white;
  border-radius: 50px;
  padding: 2px 10px;
  margin: 5px;
  font-size: large;
  font-family: "Nunito", sans-serif;
}

.pokemon-id {
  display: flex;
  justify-content: flex-end;
  color: white;
  align-items: flex-end;
  width: 100%;
  margin: 0 20px 0 0;
  font-family: "Nunito", sans-serif;
}

.pokemon-types {
  display: flex;
  gap: 10px;
}

.pokemon-type {
  background-color: white;
  border-radius: 50px;
  padding: 0px 10px;
  font-size: medium;
  font-family: "Nunito", sans-serif;
}
</style>
