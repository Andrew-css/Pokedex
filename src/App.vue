<template>
  <div id="body">
    <h1 id="poke">POKEDEX</h1>
    <button @click="ObtenerUrlPokemon()" v-if="mostrardos">Detalles</button>
    <h1>HOLA</h1>
    <div class="estadisticas" v-if="mostrar">
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
          <h1 class="barra"></h1>
          <h1>{{ attack }}</h1>
        </div>
        <div class="estadihp">
          <h1>Defense</h1>
          <h1 class="barra"></h1>
          <h1>{{ defense }}</h1>
        </div>
        <div class="estadihp">
          <h1>Special Attack</h1>
          <h1 class="barra"></h1>
          <h1>{{ specialattack }}</h1>
        </div>
        <div class="estadihp">
          <h1>Special Defense</h1>
          <h1 class="barra"></h1>
          <h1>{{ specialdefense }}</h1>
        </div>
        <div class="estadihp">
          <h1>Speed</h1>
          <h1 class="barra"></h1>
          <h1>{{ speed }}</h1>
        </div>
      </div>
      <button v-if="mostrar" @click="Volver">Volver</button>

    </div>




  </div>
</template>


<script setup>
import axios from "axios"
import { ref } from 'vue'
let img = ref()
let numero = ref()
let nombre = ref()
let hp = ref()
let attack = ref()
let defense = ref()
let specialattack = ref()
let specialdefense = ref()
let speed = ref()
let tipo_pk = ref([])
let mostrar = ref(false)
let mostrardos = ref(true)

async function ObtenerUrlPokemon() {
  /* let r = await axios.get("https://https://pokeapi.co/api/v2/pokemon?limit=50&offset=0.co/api/v2/")
  console.log(r) */

  let r = await axios.get("https://pokeapi.co/api/v2/pokemon/133")
  console.log(r)
  img.value = r.data.sprites.other["official-artwork"].front_default
  numero.value = r.data.id
  nombre.value = r.data.name
  hp.value = r.data.stats[0].base_stat
  attack.value = r.data.stats[1].base_stat
  defense.value = r.data.stats[2].base_stat
  specialattack.value = r.data.stats[3].base_stat
  specialdefense.value = r.data.stats[4].base_stat
  speed.value = r.data.stats[5].base_stat
  r.data.types.forEach(element => {
    tipo_pk.value.push(element.type.name);
  });
  mostrar.value = true
  mostrardos.value = false
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
  mostrar.value = false;
  mostrardos.value = true;
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
#poke{
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
  ;
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

#estadisticastotales{
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
  align-items: flex-end;
}

#tipopok{
  display: flex;
  gap: 30px;
}
</style>
