<template>
  <div class="container">
    <div class="container-list" v-show="show">
      <div
        v-for="pokemon in pokemons"
        v-bind:key="pokemon.name"
        class="item"
        v-on:click="selectPokemon(pokemon)"
      >
        <img v-bind:src="pokemon.image" />
        {{pokemon.name}}
      </div>
    </div>
    <div class="container-pokemon" v-on:click="back">
      <div v-show="!show" class="pokemon-item">
        <img v-bind:src="pokemon.image" />
        {{ pokemon.name }}
      </div>
    </div>
  </div>
</template>

<script>
import api from "./services/api";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      pokemon: {},
      show: true,
    };
  },

  methods: {
    selectPokemon(selectedPokemon) {
      this.pokemon = selectedPokemon;
      this.show = false;
    },

    back() {
      this.show = true;
    },
  },

  created() {
    const loadPokemons = async () => {
      const response = await api.get("api/v2/pokemon?limit=12");

      for (const pokemon of response.data.results) {
        const result = await api.get(pokemon.url);

        this.pokemons.push({
          name: result.data.name,
          image: result.data.sprites.back_default,
        });
      }
    };

    loadPokemons();
  },
};
</script>

<style>
body {
  outline: 0;
  margin: 0;
  padding: 0;
}

.container {
  background: url("./assets/background.png");
  height: 100vh;
}

.container-list {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  justify-items: center;
  align-items: center;
  height: 100vh;
  grid-gap: 8px;
}

.item {
  display: flex;
  flex-direction: column;
  padding: 32px;
  text-align: center;
  font-family: "Courier New", Courier, monospace;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 8px;
}

.container-pokemon {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.pokemon-item {
  display: flex;
  flex-direction: column;
  background: white;
  padding: 32px;
  align-items: center;
  justify-content: center;
}
</style>
