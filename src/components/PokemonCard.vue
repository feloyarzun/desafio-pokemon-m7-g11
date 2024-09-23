<script>
import axios from 'axios'

export default {
  props: ['pokemon'],
  name: 'PokemonCard',
  data() {
    return {
      image: '',
      pokemonInput: '',
      showPokemon: false
    }
  },
  async mounted() {
    try {
      const response = await axios.get(this.pokemon.url)
      this.image = response.data.sprites.front_default
    } catch (error) {
      console.error('Error', error)
    }
  },
  computed: {
    isPokemon() {
      return this.pokemonInput.trim().toLowerCase() === this.pokemon.name.toLowerCase()
    },
    imageBlur() {
      return this.showPokemon ? '' : 'filter: blur(5px) grayscale(100%);'
    }
  },
  methods: {
    checkPokemon() {
      if (this.isPokemon) {
        this.showPokemon = true
        this.$emit('increment')
      } else {
        alert('Pokemon incorrecto')
      }
    }
  }
}
</script>

<template>
  <div class="pokemon">
    <img :src="image" :style="imageBlur" />
    <div v-if="!showPokemon">
      <input v-model="pokemonInput" placeholder="Quien es este Pokemon ?" />
      <div class="btn-container">
        <button @click="checkPokemon">Descubrir</button>
      </div>
    </div>
    <div v-else>
      <p>{{ pokemon.name }}</p>
    </div>
  </div>
</template>

<style>
.pokemon {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

img {
  height: 200px;
}

.btn-container {
  margin-top: 5px;
}

button {
  width: 100px;
}

p {
  margin: 0;
  font-size: 24px;
}
</style>
