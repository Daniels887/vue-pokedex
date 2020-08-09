<template>
  <div>
    <div v-for="pokemon in pokemons" v-bind:key="pokemon.name">
      {{pokemon.name}}
    </div>
  </div>
</template>

<script>
import api from './services/api';

export default {
  name: "App",
  data(){
    return {
      pokemons: [],
    }
  },

  created() {
    api.get('api/v2/pokemon?limit=10').then(result => this.pokemons = result.data.results)
  },
};
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
