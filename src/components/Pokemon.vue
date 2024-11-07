<script setup>
/**
 * @file Pokemon.vue
 * @description Componente individual que representa una carta de Pokemon
 * Manejo de la lógica de validación de nombres y estilos visuales según estado
 */
import { ref, computed } from 'vue'



// Props recibidas del componente padre
const props = defineProps(['name', 'urlImagen']);

// Eventos emitidos al componente padre
const emit = defineEmits(['acierto']);

// Estado local del componente
const nombrePokemon = ref('')
const filtroActivo = ref(true);
const respuestaErrada = ref(false)
const respuestaCorrecta = ref(false)
const ocultarBoton = ref(false)
const aciertoNombre = ref(true)

/**
 * Valida si el nombre ingresado corresponde al Pokemon mostrado
 * @function validarNombre
 */
const validarNombre = computed(() => {
    if (nombrePokemon.value.toLowerCase() === props.name.toLowerCase()) {
        respuestaCorrecta.value = true;
        respuestaErrada.value = false;
        ocultarBoton.value = true;
        nombrePokemon.value = '';
        filtroActivo.value = false;
        aciertoNombre.value = false;

        emit('acierto');
    } else {
        respuestaErrada.value = true;
        respuestaCorrecta.value = false;
        nombrePokemon.value = '';
        alert('⚠️El nombre ingresado es incorrecto o esta vacio⚠️')
    }
});
</script>
<template>
    <!-- Carta individual de Pokemon -->
    <div class="col-12 col-md-6 col-lg-4 py-3">
        <div class="card">
            <div class="card__img">
                <!-- Imagen del Pokémon con filtro condicional -->
                <img :src="urlImagen" :class="{ filtroImagen: !respuestaCorrecta }" class="card-img-top p-3" alt="...">
            </div>
            <div class="card-body">
                <h5 v-if="respuestaCorrecta" class="card-title text-capitalize">{{ name }}</h5>
                <!-- Campo de entrada para adivinar el nombre -->
                <div class="mb-3">
                    <input v-if="!respuestaCorrecta" @keyup.enter="validarNombre" type="text"
                        :class="{ error: respuestaErrada, correcto: respuestaCorrecta, ocultar: ocultarBoton }"
                        class="form-control" id="exampleInputEmail1" aria-describedby="pokemonHelp"
                        v-model="nombrePokemon" autocomplete="nope">
                    <div id="pokemonHelp" class="form-text">Escribe el nombre del Pokémon y presiona Enter o el botón.
                    </div>
                </div>

                <button v-show="!respuestaCorrecta" @click="validarNombre" class="btn btn-primary">Descubrir</button>
            </div>
        </div>
    </div>
</template>

<style scoped>
.card {
    border: none;
    border-radius: 15px;
    overflow: hidden;
}

.card__img {
    background-image: url(../img/bg-carg-pokemon.png);
    background-size: contain;
    background-repeat: no-repeat;
    min-height: 250px;
}

.card img {
    width: 200px;
    height: 200px;
    margin: auto;
}

.filtroImagen {
    /* filter: blur(5px) grayscale(1); */
    filter: brightness(0.01);
}



@keyframes zumbido {

    0%,
    100% {
        transform: translateX(0);
    }

    20%,
    60% {
        transform: translateX(-5px);
    }

    40%,
    80% {
        transform: translateX(5px);
    }
}

.error {
    border: 2px solid red;
    animation: zumbido 0.3s ease-in-out;
}

.correcto {
    border: 2px solid green;
}

.ocultar {
    display: none;
}

.mostrarNombre {
    display: none;
}

@media (300px <=width <=1024px) {
    .card img {
        width: 150px;
        height: 150px;
    }
}
</style>