<template>
  <div class="container">
    <h1>Pokémons</h1>
    <div class="container-list" v-if="show">
      <div
        v-for="pokemon in pokemons"
        v-bind:key="pokemon.name"
        class="item"
        v-on:click="selectPokemon(pokemon)"
        v-on:keyup.enter="selectPokemon(pokemon)"
        tabindex="0"
      >
        <img v-bind:src="pokemon.image" />
        {{pokemon.name}}
      </div>
    </div>
    <div class="container-pokemon" v-else>
      <div class="pokemon-item" tabindex="0" v-on:click="back" v-on:keyup.enter="back">
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
@import url(//db.onlinewebfonts.com/c/f4d1593471d222ddebd973210265762a?family=Pokemon);

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
  grid-gap: 8px;
  height: 80vh;
  margin-top: 48px;
}

h1 {
  font-size: 72px;
  margin: 0;
  text-align: center;
  font-family: "Pokemon";
  color: yellow;
  font-weight: normal;
  letter-spacing: 4px;
}

.item {
  display: flex;
  flex-direction: column;
  padding: 32px;
  text-align: center;
  font-family: "Pokemon";
  background: rgba(255, 255, 255, 0.9);
  border-radius: 8px;
  color: rgb(173, 173, 39);
  cursor: pointer;
}

.item:focus {
  box-shadow: 0 0 1pt 3pt yellow;
  outline: 0;
}

.container-pokemon {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 80vh;
}

.pokemon-item {
  display: flex;
  flex-direction: column;
  background: white;
  padding: 32px;
  align-items: center;
  justify-content: center;
  border-radius: 8px;
  font-family: "Pokemon";
  color: rgb(173, 173, 39);
  cursor: pointer;
}

.pokemon-item:focus {
  box-shadow: 0 0 1pt 3pt yellow;
  outline: 0;
}

@media (max-width: 600px) {
  .container {
    height: 100%;
  }

  .container-list {
    padding: 32px;
    height: 100%;
  }

  .container-pokemon {
    height: calc(100vh - 93px);
  }

  .container-list {
    grid-template-columns: 1fr 1fr;
  }
}
</style>
