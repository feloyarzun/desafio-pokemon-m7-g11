<script>
import axios from 'axios'
import PokemonCard from './PokemonCard.vue'

export default {
  components: { PokemonCard },
  name: 'PokemonComponent',
  data() {
    return {
      pokemons: [],
      count: 0
    }
  },
  async mounted() {
    try {
      const response = await axios.get('https://pokeapi.co/api/v2/pokemon?=20')
      this.pokemons = response.data.results
    } catch (error) {
      console.error('Error al obtener los pokemones:', error)
    }
  },
  methods: {
    increment() {
      this.count++
    }
  }
}
</script>

<template>
  <div>
    <div class="pokemon-logo">
      <img src=".././assets/pokemon.PNG" />
    </div>
    <h1>¿Quién es ese Pokemon?</h1>
    <p>
      Pokemones descubiertos:
      <span class="count">
        {{ count }}
      </span>
    </p>
    <div class="card">
      <PokemonCard
        v-for="(pokemon, index) in pokemons"
        :key="index"
        :pokemon="pokemon"
        @increment="increment"
      />
    </div>
  </div>
</template>

<style>
h1,
p {
  text-align: center;
}

.count {
  color: #daa520;
  font-size: 20px;
  font-weight: bold;
}

.card {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 40px;
}

img {
  width: 35%;
}

.pokemon-logo {
  display: flex;
  justify-content: center;
}
</style>
