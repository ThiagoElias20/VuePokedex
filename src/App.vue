<template>
  <v-app>
      <PokemonItem v-for="pokemon in pokemons.results" :key="pokemon.name" :pokemon="pokemon" />
  </v-app>
</template>

<script>
import axios from 'axios';
import PokemonItem from './components/PokemonItem.vue'

export default {
  name: 'App',
  components: {
    PokemonItem
  },

  data() {
    return {
      pokemons: [],
    }
  },

  mounted() {
    axios.get("https://pokeapi.co/api/v2/pokemon/?limit=1302").then(response => {
      this.pokemons = response.data;
    })
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
