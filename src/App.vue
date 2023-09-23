<template>
  <div id="body">
    <div v-if="mostrar">
      <h1 id="poke">POKEDEX</h1>
      <div v-for="(pokemon, index) in pokemonList" :key="index" class="pokemon-card">
        <h1>#{{ pokemon.numero }}</h1>
        <img :src="pokemon.img" alt="">
        <h1>{{ pokemon.nombre }}</h1>
        <h1>Tipo: {{ pokemon.tipo_pk.join(', ') }}</h1>
        <button @click="ObtenerUrlPokemon(pokemon)">Detalles</button>
      </div>
    </div>
    <div class="estadisticas" v-if="mostrardos">
      <h1>POKEDEX</h1>
      <div id="contenidopokemon">
        <div id="contenido2">
          <h1 id="numeropok"> #{{ numero }}</h1>
          <img :src="img" alt="">
          <h1 id="nombrepok">{{ nombre }}</h1>
        </div>
      </div>
      <div id="tipopok">
        <h1 v-for="(item, index) in tipo_pk" :key="index">{{ item }}</h1>
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
import { ref, onMounted } from 'vue'

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

#poke {
  font-size: 50px;
}

.estadihp {
  display: flex;
  justify-content: center;
  gap: 50px;
}

img {
  width: 300px;
  height: 300px;
  background-color: black;
}

button {
  width: 200px;
  font-size: 25px;
  padding: 15px;
  font-weight: bolder;
}

button:hover {
  background-color: rgb(21, 165, 231);
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
}

#contenido2 {
  display: flex;
  gap: 40px;
  align-items: center;
}

#numeropok {
  color: rgb(251, 255, 13);
  font-size: 70px;
}

#nombrepok {
  color: rgb(251, 255, 13);
  font-size: 70px;
}

#estadisticastotales {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
  align-items: flex-end;
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
  background-color: #f5f5f5;
}

.pokemon-card img {
  width: 150px;
  height: 150px;
}

.pokemon-card h1 {
  font-size: 24px;
  color: #333;
}
</style>
