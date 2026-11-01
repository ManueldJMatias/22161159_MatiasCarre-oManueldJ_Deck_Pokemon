
<script setup>
import { computed } from 'vue'

const props = defineProps({
  pokemon: {
    type: Object,
    required: true
  }
})

const tipo = computed(() => props.pokemon.types[0].type.name)

const colores = {
  fire: '#ef7448',
  water: '#4e9cdb',
  grass: '#69b96b',
  electric: '#edc84a',
  psychic: '#c77cbe',
  ice: '#70cbd4',
  fighting: '#c57857',
  poison: '#a47ac5',
  ground: '#c8a66b',
  flying: '#8da7e8',
  bug: '#92b44e',
  rock: '#ac9d80',
  ghost: '#8177b2',
  dragon: '#8273d9',
  dark: '#777783',
  steel: '#8da5b0',
  fairy: '#e5a0c8',
  normal: '#a5a59b'
}

const color = computed(() => colores[tipo.value] || '#999')

const imagen = computed(() =>
  props.pokemon.sprites.other['official-artwork'].front_default ||
  props.pokemon.sprites.front_default
)

const movimientos = computed(() =>
  props.pokemon.moves.slice(0, 2)
)

function traducirTipo(tipo) {
  const nombres = {
    fire: 'Fuego', water: 'Agua', grass: 'Planta',
    electric: 'Eléctrico', normal: 'Normal',
    psychic: 'Psíquico', ice: 'Hielo',
    fighting: 'Lucha', poison: 'Veneno',
    ground: 'Tierra', flying: 'Volador',
    bug: 'Bicho', rock: 'Roca',
    ghost: 'Fantasma', dragon: 'Dragón',
    dark: 'Siniestro', steel: 'Acero',
    fairy: 'Hada'
  }
  return nombres[tipo] || tipo
}

function formato(nombre) {
  return nombre.replaceAll('-', ' ')
}
</script>

<template>
  <article class="pokemon-card" :style="{ '--tipo-color': color }">
    <div class="carta-interior">
      <div class="encabezado">
        <div>
          <span class="etiqueta">POKÉMON</span>
          <h2>{{ pokemon.name }}</h2>
          <small>#{{ String(pokemon.id).padStart(3, '0') }}</small>
        </div>
        <strong class="hp">{{ pokemon.stats[0].base_stat }} HP</strong>
      </div>

      <div class="imagen-contenedor">
        <img :src="imagen" :alt="pokemon.name" />
      </div>

      <div class="tipo">
        <span v-for="t in pokemon.types" :key="t.type.name">
          {{ traducirTipo(t.type.name) }}
        </span>
      </div>

      <div class="caracteristicas">
        <p><strong>Altura:</strong> {{ pokemon.height / 10 }} m</p>
        <p><strong>Peso:</strong> {{ pokemon.weight / 10 }} kg</p>
        <p><strong>Ataque:</strong> {{ pokemon.stats[1].base_stat }}</p>
        <p><strong>Defensa:</strong> {{ pokemon.stats[2].base_stat }}</p>
      </div>

      <div class="habilidades">
        <h3>Movimientos</h3>
        <div v-for="(mov, index) in movimientos"
             :key="mov.move.name" class="movimiento">
          <span>{{ index + 1 }}. {{ formato(mov.move.name) }}</span>
          <span>✦</span>
        </div>
      </div>

      <div class="pie">POKÉMON DECK · VUE.JS</div>
    </div>
  </article>
</template>

<style scoped>
.pokemon-card {
  background: var(--tipo-color);
  border: 5px solid #e9c75d;
  border-radius: 18px;
  padding: 8px;
  box-shadow: 0 7px 18px #0003;
  transition: transform .25s, box-shadow .25s;
  min-width: 0;
}

.pokemon-card:hover {
  transform: translateY(-7px);
  box-shadow: 0 13px 25px #0004;
}

.carta-interior {
  background: linear-gradient(145deg, #ffffffdd, #ffffff9c);
  border-radius: 10px;
  padding: 12px;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.encabezado {
  display: flex;
  justify-content: space-between;
  align-items: start;
  gap: 8px;
}

.encabezado h2 {
  text-transform: capitalize;
  font-size: 22px;
  margin: 3px 0;
}

.etiqueta, .encabezado small {
  font-size: 10px;
  font-weight: bold;
}

.hp { color: #bd3030; white-space: nowrap; }

.imagen-contenedor {
  height: 175px;
  background: radial-gradient(circle, #fff, #ffffff55);
  border: 3px solid #d6bd68;
  border-radius: 9px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 12px 0;
}

.imagen-contenedor img {
  width: 95%;
  height: 95%;
  object-fit: contain;
}

.tipo { display: flex; gap: 6px; flex-wrap: wrap; }

.tipo span {
  background: var(--tipo-color);
  padding: 5px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: bold;
  color: white;
}

.caracteristicas {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 6px;
  margin: 13px 0;
  font-size: 12px;
}

.caracteristicas p { margin: 0; }

.habilidades {
  border-top: 2px solid #0002;
  padding-top: 8px;
  margin-top: auto;
}

.habilidades h3 {
  margin: 0 0 8px;
  font-size: 15px;
}

.movimiento {
  display: flex;
  justify-content: space-between;
  text-transform: capitalize;
  padding: 6px 0;
  border-bottom: 1px solid #0002;
  font-size: 12px;
}

.pie {
  text-align: center;
  font-size: 9px;
  margin-top: 12px;
  opacity: .6;
}
</style>
