<script>
import PokemonCard from "../components/PokemonCard.vue";
import axios from "axios";
export default {
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemons: [],
      pokemonadivinados: [],
    };
  },
  async mounted() {
    const response = await axios.get(
      "https://pokeapi.co/api/v2/pokemon?limit=20"
    );
    const pokemonResults = response.data.results;
    console.log(pokemonResults);
    const pokemonData = await axios.all(
      pokemonResults.map((pokemon) => axios.get(pokemon.url))
    );
    console.log(pokemonData);
    this.pokemons = pokemonData.map((pokemon) => pokemon.data);
    console.log(this.pokemons);
  },
  methods: {
    sumaContador(adivinado) {
      if (adivinado) {
        this.pokemonadivinados.push(adivinado);
      }
    },
  },
  computed: {
    contador() {
      return this.pokemonadivinados.length;
    },
  },
};
</script>

<template>
  <h2>Pokémon descubiertos: {{ contador }}</h2>
  <main>
    <div v-for="pokemon in pokemons" :key="pokemon">
      <PokemonCard :pokemon="pokemon" @pokemonAdivinado="sumaContador" />
    </div>
  </main>
</template>

<style>
h2 {
  text-align: center;
  color: white;
}
main {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;  
}
</style>
