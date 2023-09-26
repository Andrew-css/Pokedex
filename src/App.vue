<template>
  <div id="body">
    <div v-if="mostrar" class="Primero">
      <div id="Encabezado">
        <div id="encabezao">
          <h1 id="poke">POKEDEX</h1>
        </div>
        <div class="input__container">
          <div class="shadow__input"></div>
          <button class="input__button__shadow">
            <svg fill="none" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" height="20px" width="20px">
              <path
                d="M4 9a5 5 0 1110 0A5 5 0 014 9zm5-7a7 7 0 104.2 12.6.999.999 0 00.093.107l3 3a1 1 0 001.414-1.414l-3-3a.999.999 0 00-.107-.093A7 7 0 009 2z"
                fill-rule="evenodd" fill="#17202A"></path>
            </svg>
          </button>
          <input v-model="searchTerm" type="text" name="text" class="input__search" placeholder="Buscar pókemon...">
        </div>
      </div>
      <div class="ContainerTarjetas">
        <div v-for="(pokemon, index) in filteredPokemonList" :key="index" :class="getPokemonCardClasses(pokemon)" class="pokemon-card">
          <h1>#{{ pokemon.numero }}</h1>
          <img :src="pokemon.img" alt="">
          <h1>{{ pokemon.nombre }}</h1>
          <h1>Tipo: {{ pokemon.tipo_pk.join(', ') }}</h1>
          <button @click="ObtenerUrlPokemon(pokemon)">Detalles</button>
        </div>
      </div>
      <div>
        <button @click="cargarMasPokemones(50)">Cargar más...</button>
      </div>

    </div>
    <div class="estadisticas" v-if="mostrardos">
      <div id="contenidopokemon">
        <div id="contenido2">
          <img :src="img" alt="" id="imgDetalle">
        </div>
          <div id="contenido21">
            <h1 id="numeropok"> #{{ numero }}</h1>
            <h1 id="nombrepok" class="nombrepokemon">{{ nombre }}</h1>
          </div>
      </div>
      <div id="tipopok">
        <h1 v-for="(item, index) in tipo_pk" :key="index" style=" text-transform:capitalize;">{{ item }}</h1>
      </div>

      <div id="estat">
        <div id="esta">
          <h1>Estadisticas</h1>
        </div>
      </div>
      <div id="estadisticastotales">
        <div class="estadihp">
          <h1>Hp</h1>
          <div class="barrita">
            <h1 class="barra1">.</h1>
          </div>
          <h1>{{ hp }}</h1>
        </div>
        <div class="estadihp">
          <h1>Attack</h1>
          <div class="barrita">
            <h1 class="barra1">.</h1>
          </div>
          <h1>{{ attack }}</h1>
        </div>
        <div class="estadihp">
          <h1>Defense</h1>
          <div class="barrita">
            <h1 class="barra1">.</h1>
          </div>
          <h1>{{ defense }}</h1>
        </div>
        <div class="estadihp">
          <h1>Special Attack</h1>
          <div class="barrita">
            <h1 class="barra1">.</h1>
          </div>
          <h1>{{ specialattack }}</h1>
        </div>
        <div class="estadihp">
          <h1>Special Defense</h1>
          <div class="barrita">
            <h1 class="barra1">.</h1>
          </div>
          <h1>{{ specialdefense }}</h1>
        </div>
        <div class="estadihp">
          <h1>Speed</h1>
          <div class="barrita">
            <h1 class="barra1">.</h1>
          </div>
          <h1>{{ speed }}</h1>
        </div>
      </div>
      <button v-if="mostrardos" @click="Volver">Volver</button>
    </div>
  </div>
</template>

<script setup>
import axios from "axios"
import { ref, onMounted, computed } from 'vue'

let img = ref('')
let numero = ref('')
let nombre = ref('')
let hp = ref('')
let attack = ref('')
let defense = ref('')
let specialattack = ref('')
let specialdefense = ref('')
let speed = ref('')
let tipo_pk = ref([])
let mostrar = ref(true)
let mostrardos = ref(false)
let pokemonList = ref([]);
let searchTerm = ref('');

const filteredPokemonList = computed(() => {
  return pokemonList.value.filter((pokemon) => {
    return pokemon.nombre.toLowerCase().includes(searchTerm.value.toLowerCase());
  });
});

async function ObtenerUrlPokemon(pokemon) {
  const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemon.numero}`)
  const data = response.data

  img.value = data.sprites.other["official-artwork"].front_default
  numero.value = data.id
  nombre.value = data.name
  hp.value = data.stats[0].base_stat
  attack.value = data.stats[1].base_stat
  defense.value = data.stats[2].base_stat
  specialattack.value = data.stats[3].base_stat
  specialdefense.value = data.stats[4].base_stat
  speed.value = data.stats[5].base_stat

  tipo_pk.value = data.types.map((element) => element.type.name)

  mostrar.value = false
  mostrardos.value = true
}

function Volver() {
  img.value = '';
  numero.value = '';
  nombre.value = '';
  hp.value = '';
  attack.value = '';
  defense.value = '';
  specialattack.value = '';
  specialdefense.value = '';
  speed.value = '';
  tipo_pk.value = [];
  mostrar.value = true;
  mostrardos.value = false;
}

onMounted(() => {
  obtenerPokemones()
})

async function obtenerPokemones() {
  for (let i = 1; i <= 50; i++) {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`)
    const data = response.data

    pokemonList.value.push({
      img: data.sprites.other["official-artwork"].front_default,
      numero: data.id,
      nombre: data.name,
      hp: data.stats[0].base_stat,
      attack: data.stats[1].base_stat,
      defense: data.stats[2].base_stat,
      specialattack: data.stats[3].base_stat,
      specialdefense: data.stats[4].base_stat,
      speed: data.stats[5].base_stat,
      tipo_pk: data.types.map((element) => element.type.name),
    })
  }
}

async function cargarMasPokemones(cantidad) {
  const startIndex = pokemonList.value.length + 1;
  const endIndex = startIndex + cantidad - 1;

  for (let i = startIndex; i <= endIndex; i++) {
    const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${i}`)
    const data = response.data

    pokemonList.value.push({
      img: data.sprites.other["official-artwork"].front_default,
      numero: data.id,
      nombre: data.name,
      hp: data.stats[0].base_stat,
      attack: data.stats[1].base_stat,
      defense: data.stats[2].base_stat,
      specialattack: data.stats[3].base_stat,
      specialdefense: data.stats[4].base_stat,
      speed: data.stats[5].base_stat,
      tipo_pk: data.types.map((element) => element.type.name),
    })
  }
}





function getPokemonCardClasses(pokemon) {
  const classes = pokemon.tipo_pk.map((tipo) => tipo.toLowerCase()) // Convierte los tipos a minúsculas
  return classes.join(' ')
}



</script>

<style scoped>
#body {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  gap: 20px;
  align-items: center;
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}

#Encabezado {
  display: flex;
  flex-direction: row;
  width: 100%;
  align-items: center;
  justify-content: center;
  gap: 200px;
}

#encabezao {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.input__container {
  position: relative;
  background: rgba(255, 255, 255, 0.664);
  padding: 10px 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5px;
  border-radius: 22px;
  max-width: 300px;
  transition: transform 400ms;
  transform-style: preserve-3d;
  perspective: 500px;
  margin-right: 150px;
  width: 100%;
}

.shadow__input {
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  left: 0;
  bottom: 0;
  z-index: -1;
  filter: blur(30px);
  border-radius: 20px;
  background-color: #999cff;
  background-image: radial-gradient(at 85% 51%, hsla(60, 60%, 61%, 1) 0px, transparent 50%),
    radial-gradient(at 74% 68%, hsla(235, 69%, 77%, 1) 0px, transparent 50%),
    radial-gradient(at 64% 79%, hsla(284, 72%, 73%, 1) 0px, transparent 50%),
    radial-gradient(at 75% 16%, hsla(283, 60%, 72%, 1) 0px, transparent 50%),
    radial-gradient(at 90% 65%, hsla(153, 70%, 64%, 1) 0px, transparent 50%),
    radial-gradient(at 91% 83%, hsla(283, 74%, 69%, 1) 0px, transparent 50%),
    radial-gradient(at 72% 91%, hsla(213, 75%, 75%, 1) 0px, transparent 50%);
  
}

.input__button__shadow {
  cursor: pointer;
  border: none;
  background: none;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 12px;
  padding: 5px;
  width: 50px;
}

.input__button__shadow:hover {
  background: rgba(255, 255, 255, 0.411);
}

.input__search {
  width: 100%;
  border-radius: 20px;
  outline: none;
  border: none;
  padding: 8px;
  position: relative;
}

#poke {
  font-size: 80px;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  color: rgb(255, 255, 255);
}

.estadihp {
  display: flex;
  justify-content: center;
  gap: 50px;
}

img {
  border-radius: 30px;
}

button {
  width: 200px;
  font-size: 25px;
  padding: 15px;
  font-weight: bolder;
  border-radius: 10px;
}

button:hover {
  background-color: rgb(21, 165, 231);
}

.ContainerTarjetas {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

}

.Primero {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.estadisticas {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
  align-items: center;
}

.barra {
  background-color: rgb(0, 110, 118);
  width: 800px;
  border-radius: 15px;
}

#imgDetalle {
  width: 450px;
  height: 450px;
 
}

.barra1 {
  background-color: rgb(0, 110, 118);
  width: 800px;
  border-radius: 15px;
  color: rgb(0, 110, 118);
}

#contenidopokemon {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.barrita {
  width: 800px;
}

#esta {
  display: flex;
  width: 100%;
  font-size: 20px;
}

#contenido2 {
  display: flex;
  gap: 40px;
  align-items: center;
  margin-right: 300px;
}
  

#contenido21{
  display: grid;
  width: 100%
}

#numeropok {
  color: rgb(255, 255, 255);
  font-size: 100px;
}

#nombrepok {
  color: rgb(255, 255, 255);
  font-size: 90px;
  text-transform: capitalize;
}

#estadisticastotales {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
  align-items: flex-end;
  font-size: 17px;
}

#tipopok {
  display: flex;
  gap: 30px;
}

.pokemon-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 2px solid #ddd;
  border-radius: 10px;
  padding: 10px;
  margin: 10px;
  background: linear-gradient(to bottom, rgba(255, 255, 255, 0.5), rgba(255, 255, 255, 0.8));
  background-size: cover;
  width: 250px;
  text-transform: capitalize;


}

.pokemon-card img {
  width: 150px;
  height: 150px;
}

.pokemon-card h1 {
  font-size: 24px;
  color: #333;
}
/* Tipos de Pokémon */
.water {
  background-color: blue;
}

.grass {
  background-color: green;
}

.poison {
  background-color: purple;
}

.fire {
  background-color: red;
}

.flying {
  background-color: gray;
}

.bug {
  background-color: brown;
}

.normal {
  background-color: white;
}

.electric {
  background-color: yellow;
}

.ground {
  background-color: black;
}

.fairy {
  background-color: pink;
}

/* Clases para combinaciones de tipos */
.water.grass {
  background: linear-gradient(to bottom, blue, green);
}

.grass.poison {
  background: linear-gradient(to bottom, green, purple);
}

.fire.flying {
  background: linear-gradient(to bottom, red, gray);
}

.poison.ground {
  background: linear-gradient(to bottom, purple, black);
}

.poison.flying {
  background: linear-gradient(to bottom, purple, gray);
}

.bug.grass {
  background: linear-gradient(to bottom, brown, green);
}

.bug.poison {
  background: linear-gradient(to bottom, brown, purple);
}

.normal.flying {
  background: linear-gradient(to bottom, white, gray);
}

.bug.flying {
  background: linear-gradient(to bottom, brown, gray);
}

.fire.flying.pokemon-card h1 {
  color: white;
}
.grass.poison.pokemon-card h1 {
  color: white;
}

.poison.ground.pokemon-card h1{
  color: white;
}

.poison.flying.pokemon-card h1 {
 color: white
}

.bug.grass.pokemon-card h1 {
  color: white;
}

.bug.poison.pokemon-card h1 {
  color: white;
}

.bug.flying.pokemon-card h1 {
  color: white;
}
</style>
