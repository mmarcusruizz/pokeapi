<template>
  <div id="app">
    <img src="./assets/plogo.svg" alt="Imagen Pokémon" class="pokemon-logo">
    <h1>¿Quién es ese Pokémon?</h1>
    <ul>
      <!-- Itera sobre la lista de Pokémon y renderiza un componente PokemonView para cada uno -->
      <li v-for="pokemon in pokemonList" :key="pokemon.name">
        <PokemonView :pokemon="pokemon" @discovered="incrementScore" />
      </li>
    </ul>
    <p>Pokemones descubiertos: {{ score }}</p>
  </div>
</template>

<script>
import PokemonView from "./components/PokemonView.vue";

export default {
  name: "App",
  components: {
    PokemonView,
  },
  data() {
    return {
      pokemonList: [], // Almacena la lista de Pokémon obtenida de la API
      score: 0,
    };
  },
  created() {
    this.fetchPokemonList();
  },
  methods: {
     // Función para obtener la lista de Pokémon de la API
    async fetchPokemonList() {
      try {
        // Realiza una petición a la API de Pokémon para obtener los primeros 20 Pokémon
        const response = await fetch(
          "https://pokeapi.co/api/v2/pokemon?limit=20"
        );
        const data = await response.json();
        // Itera sobre los resultados y obtiene los detalles de cada Pokémon
        this.pokemonList = await Promise.all(
          data.results.map(async (pokemon) => {
            const res = await fetch(pokemon.url);
            const details = await res.json();
            return {
              name: details.name,
              image: details.sprites.front_default,
            };
          })
        );
      } catch (error) {
        console.error("Error al obtener los pokemon:", error);
      }
    },
    // Función para incrementar el puntaje
    incrementScore() {
      this.score++;
    },
  },
};
</script>

<style>
.pokemon-logo {
  display: block;
  margin: 0 auto;
  max-width: 200px;
}
h1 {
  font-size: 40px;
  color: #007bff;
}

#app {
  font-family: Helvetica, sans-serif;
  text-align: center;
}

/* Pokemon List */
.pokemon-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 2px;
}

ul {
  list-style-type: none;
  padding: 20px;
}

li {
  display: inline-block;
  margin: 20px;
  word-break: break-all;
}
</style>
