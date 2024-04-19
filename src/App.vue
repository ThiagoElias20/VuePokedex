<template>
  <div class="pokemon-list" @scroll="lidarScroll">
    <v-container>
      <v-row>
        <v-col v-for="(pokemon) in PokemonsVisiveis" :key="pokemon.name" :cols="3">
          <PokemonItem :pokemon="pokemon" />
        </v-col>
      </v-row>
    </v-container>
  </div>
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
      pokemons: [], //armazena pokemons carregados
      PokemonsVisiveis: [], //armazena os pokemons visiveis na tela
      offset: 0, //determina a partir de que ponto devem ser carregados novos dados
      limit: 18, // número de pokemons por carregamento
    }
  },

  mounted() {
    this.carregarNovosPokemons(); //carrega os primeiros pokemons
    window.addEventListener('scroll', this.lidarScroll); //ativa o lidarScroll quando ocorrer evento de scroll
  },

  methods: {
    carregarNovosPokemons() { //pede a PokeAPI para pegar uma lista nova de pokemons, se baseando no offseat e no limite
      axios.get(`https://pokeapi.co/api/v2/pokemon/?offset=${this.offset}&limit=${this.limit}`).then(response => {
        this.pokemons.push(...response.data.results); //Adiciona a array pokemons
        this.offset += this.limit; //pega os novos pokemons do offset até o limit
        this.updatePokemonsVisiveis();
      })
    },

    updatePokemonsVisiveis() {
      const startIndex = this.PokemonsVisiveis.length; //indice inicial
      const endIndex = startIndex + this.limit; //indice final
      this.PokemonsVisiveis.push(...this.pokemons.slice(startIndex, endIndex)); //adiciona os pokemons no intervalo da array pokemons
    },

    lidarScroll() {
      const scrollHeight = document.documentElement.scrollHeight;
      const scrollTop = document.documentElement.scrollTop;
      const clientHeight = document.documentElement.clientHeight; //altura da janela

      if (scrollTop + clientHeight >= scrollHeight - 200) { //acionada quando o usuário "scrolla" a página
        this.carregarNovosPokemons();
      }
    }
  },

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
