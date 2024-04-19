<template>
    <v-container>
      <img :src="getPokemonImageUrl(pokemon.url)" :alt="pokemon.name"> <!--Img do Pokemon-->
      <h2>{{ pokemon.name }}</h2> <!--Nome do Pokemon-->
    </v-container>
</template>

<script>
export default {
  props: {
    pokemon: {
      type: Object,
      default: () => ({})
    }
  },

  methods: {
    getPokemonImageUrl(url) {
      const id = url.split('/').slice(-2, -1)[0];
      return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${id}.png`;
    }
  }
}
</script>

<style scoped>

</style>