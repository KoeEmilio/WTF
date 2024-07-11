<script setup>
import { ref } from 'vue';
import { pokeapi } from "@/api/pokeapi.js";

const PokemonData = ref({});
const PokemonId = ref('');

const buscarpokemon = async () => {
  try {
    const response = await fetch(`${pokeapi}/${PokemonId.value}`);
    if (!response.ok) {
      throw new Error('No se encontró el pokemon');
    }
    const data = await response.json();
    PokemonData.value = data;
    console.log(data);
    return data;
  } catch (error) {
    alert(error.message);
  }
};
</script>

<template>
<div class="contenedor">
  <div class="barra">
    <label>
    Ingresa el numero del pokemon:
    <input type="text"  v-model="PokemonId">
  </label>
  <button class="Search" @click="buscarpokemon">Buscar</button>
  </div>
  <div class="PokeCard">
    <h1 class="pokename">{{ PokemonData.name }}</h1>
    <img :src="PokemonData.sprites?.front_default" :alt="PokemonData.name">
    <ul class="tipo">
      <h2>Tipo:</h2>
      <li v-for="(type, index) in PokemonData.types" :key="index">
        <span>{{ type.type.name }}</span>
      </li>
    </ul>
  </div>
  <div class="stats">
    <h1>Stats:</h1>
    <br>
    <ul>
      <li v-for="(stat, index) in PokemonData.stats" :key="index">
        <span>{{ stat.stat.name }} ->  <span>{{ stat.base_stat }}</span></span>
      </li>
    </ul>
  </div>
</div>
</template>

<style>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.contenedor{
  display: grid;
  grid-template-rows: 100px 1fr;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-areas:'menu menu menu menu'
                      'main main aside aside';
  height: 100vh;
  width: 100vw;
  background-color: rgb(16, 16, 168);
}

.barra{
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: black;
  color: white;
  grid-area: menu;
}

.barra input{
  margin-left: 20px;
  margin-right: 20px;
}

.PokeCard{
background-color: rgb(211, 11, 55);
grid-area: main;
}

.stats{
background-color: white;
grid-area: aside;
}

.PokeCard{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  color: white;
  text-align: center;
  font-size: larger;
}

.PokeCard img{
  margin-left: 130px;
  height: 400px;
  width: 400px;
}

.stats{
  text-align: center;
  font-size: xx-large;
  text-align: center;
  align-items: center;
}
</style>

