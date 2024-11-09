<template>
  <h2 v-if="juegoIniciado">Pokémon descubiertos: <span class="contador">{{ contador }} </span> </h2>
  <main>
    <button v-if="!juegoIniciado" @click="iniciarJuego">Iniciar Juego</button>

    <div v-if="juegoIniciado" v-for="pokemon in pokemons" :key="pokemon.id">
      <PokemonCard 
        :pokemon="pokemon" 
        @pokemonAdivinado="sumaContador" 
        @pokemonOmitido="omitirPokemon"
      />
    </div>

    <!-- Modal de Resultados -->
    <ModalResultado 
      v-if="modalVisible"
      :pokemonadivinados="pokemonadivinados"
      :pokemonOmitidos="pokemonOmitidos"
      @cerrarModal="cerrarModal"
    />
  </main>
</template>

<script>
import PokemonCard from "../components/PokemonCard.vue";
import ModalResultado from "../components/ModalResultado.vue";
import axios from "axios";

export default {
  components: {
    PokemonCard,
    ModalResultado,
  },
  data() {
    return {
      pokemons: [],
      pokemonadivinados: [],
      pokemonOmitidos: [],
      juegoIniciado: false,
      modalVisible: false, // Controla la visibilidad del modal
      pokemonContador: 0, // Variable para contar adivinados y omitidos
    };
  },
  methods: {
    async iniciarJuego() {
      this.juegoIniciado = true;
      this.pokemonadivinados = []; // Reiniciar el contador de adivinados
      this.pokemonOmitidos = []; // Reiniciar el contador de omitidos
      this.pokemonContador = 0; // Reiniciar el contador
      this.modalVisible = false; // Asegurarse de que el modal esté cerrado al reiniciar

      // Genera ids aleatorios
      const randomIds = new Set();
      while (randomIds.size < this.rondasPorAdivinar) {
        randomIds.add(Math.floor(Math.random() * (endId - startId + 1)) + startId);//toma un número aleatorio entre el rango de ids
      }

      const idsArray = Array.from(randomIds);

      const pokemonData = await axios.all(
        idsArray.map((id) =>
          axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
        )
      );

      // Asigna los datos obtenidos al array pokemons
      this.pokemons = pokemonData.map((pokemon) => pokemon.data);
    },
    sumaContador(adivinado) {
      if (adivinado) {
        this.pokemonadivinados.push(adivinado);
        this.pokemonContador++; // Aumentar el contador de Pokémon adivinados
      }
      this.checkRondaTerminada();
    },
    omitirPokemon(pokemon) {
      this.pokemonOmitidos.push(pokemon);
      this.pokemonContador++; // Aumentar el contador de Pokémon omitidos
      this.checkRondaTerminada();
    },
    checkRondaTerminada() {
      // Verifica si se han adivinado o omitido los 20 Pokémon
      if (this.pokemonContador === this.rondasPorAdivinar) {// Cambiar el número de rondas por dificultad
        this.modalVisible = true; // Mostrar el modal cuando todos los Pokémon hayan sido respondidos
      }
    },
    cerrarModal() {
      // Reiniciar el estado del juego sin recargar la página
      this.juegoIniciado = false;
      this.pokemons = [];
      this.pokemonadivinados = [];
      this.pokemonOmitidos = [];
      this.pokemonContador = 0; // Reiniciar el contador
      this.modalVisible = false;
    }
  },
  computed: {
    contador() {
      return this.pokemonadivinados.length;
    },
  },
};
</script>

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
button {
  margin: 40px auto;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: rgb(195, 11, 11);
  color: white;
}
button:hover {
  background-color: rgb(255, 0, 0);
}

.contador {
  color: #ee830a;
  font-size: 40px;
  font-weight: bold;
}
</style>
