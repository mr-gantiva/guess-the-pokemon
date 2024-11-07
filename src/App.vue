<script setup>
/**
 * @file App.vue
 * @description Componente principal del juego de Adivinanza de Pokemon
 * Se obtienen los datos mediante API y se genera un contador global de aciertos
 */
import { ref, onMounted } from 'vue';
import axios from 'axios';
import Pokemon from './components/Pokemon.vue';

// URL base para las imagenes de Pokemon en formato SVG
const urlImagen = ref('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/')

// Estado global de la aplicación
const personajes = ref([]);
const contadorFinal = ref(0);

/**
 * Obtiene la lista de Poñemon desde la PokeAPI
 * @async
 * @function getPersonajes
 * @returns {Promise<void>}
 */
const getPersonajes = async () => {
  try {
    const url = "https://pokeapi.co/api/v2/pokemon?limit=20&offset=0";
    const { data } = await axios.get(url);
    personajes.value = data.results;
  } catch (error) {
    console.log(error)
  }
}

/**
 * Incrementa el contador global cuando un Pokemon es acertado
 * @function incrementarContador
 */

const incrementarContador = () => {
  contadorFinal.value++; // Actualiza el contador final con el nuevo valor
};

// Carga inicial de datos cuanto el componente se monta
onMounted(getPersonajes());
</script>

<template>
  <div class="container py-5">
    <div class="logo  d-flex flex-column justify-content-center align-items-center">
      <img src="./img/pokemon-logo.png" alt="" class="w-50">
    </div>

    <!-- Muestra el contador global de aciertos -->
    <div class="d-flex justify-content-end contenedor__aciertos">
      <h4 class="p-3 text-end">Aciertos: {{ contadorFinal }}</h4>
    </div>
  </div>


  <!-- Contenedor principal del juego -->
  <section class="container pb-5">
    <div class="row">
      <!-- Renderiza un componente Pokemon por cada personaje en la lista -->
      <Pokemon v-for="personaje in personajes" :key="personaje.name" :name="personaje.name"
        :urlImagen="urlImagen + personaje.url.split('/')[6] + '.svg'" @acierto="incrementarContador">
      </Pokemon>
    </div>
  </section>

</template>

<style scoped>
.contenedor__aciertos h4 {
  background-color: #2773BA;
  color: #FFCC01;
  border-radius: 5px;
  border: 3px solid #fafaf5;
}
</style>
