
<script setup>
import { ref, onMounted } from 'vue'
import PokemonCard from './components/PokemonCard.vue'

const pokemones = ref([])
const cargando = ref(false)
const error = ref('')

function generarIds() {
  const ids = new Set()

  while (ids.size < 40) {
    ids.add(Math.floor(Math.random() * 1025) + 1)
  }

  return [...ids]
}

async function cargarPokemones() {
  cargando.value = true
  error.value = ''
  pokemones.value = []

  try {
    const ids = generarIds()

    const resultados = await Promise.all(
      ids.map(async (id) => {
        const respuesta = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${id}`
        )

        if (!respuesta.ok) {
          throw new Error(`Error al obtener Pokémon #${id}`)
        }

        return respuesta.json()
      })
    )

    pokemones.value = resultados
  } catch (e) {
    error.value = 'No se pudieron cargar los Pokémon. Intenta de nuevo.'
    console.error(e)
  } finally {
    cargando.value = false
  }
}

onMounted(cargarPokemones)
</script>

<template>
  <main>
    <header class="hero">
      <div class="pokebola">◉</div>
      <h1>POKÉMON DECK</h1>
      <p>Tu colección de 40 cartas Pokémon</p>

      <button @click="cargarPokemones" :disabled="cargando">
        {{ cargando ? 'Cargando...' : 'Generar nuevo deck' }}
      </button>
    </header>

    <section class="contenido">
      <div class="barra">
        <h2>Mi colección</h2>
        <span>{{ pokemones.length }} / 40 cartas</span>
      </div>

      <div v-if="cargando" class="mensaje">
        Cargando las 40 cartas Pokémon...
      </div>

      <div v-else-if="error" class="mensaje">
        {{ error }}
        <button @click="cargarPokemones">Reintentar</button>
      </div>

      <div v-else class="grid-cartas">
        <PokemonCard
          v-for="pokemon in pokemones"
          :key="pokemon.id"
          :pokemon="pokemon"
        />
      </div>
    </section>

    <footer>
      Proyecto de Servicios Web · Vue.js + PokéAPI
    </footer>
  </main>
</template>
