<template>
  <h2 v-if="juegoIniciado">Pokémon descubiertos: <span class="contador">{{ contador }} </span> </h2>
  <main>
    
    <button v-if="!juegoIniciado" @click="iniciarJuego">Iniciar Juego</button>

    <div v-if="juegoIniciado">
      <PokemonCard 
        v-for="(pokemon, index) in pokemons"
        :key="pokemon.id"
        :pokemon="pokemon"
        v-show="index === tarjetaActual"  
        @pokemonAdivinado="sumaContador"
        @pokemonOmitido="omitirPokemon"
        @avanzarCard="avanzarCard"  
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
      tarjetaActual: 0, // Índice de la tarjeta actual
    };
  },

  methods: {
    async iniciarJuego() {
      this.juegoIniciado = true;
      this.pokemonadivinados = []; // Reiniciar el contador de adivinados
      this.pokemonOmitidos = []; // Reiniciar el contador de omitidos
      this.pokemonContador = 0; // Reiniciar el contador
      this.tarjetaActual = 0; // Comenzar desde la primera tarjeta
      this.modalVisible = false; // Asegurarse de que el modal esté cerrado al reiniciar

      // Genera ids aleatorios
      const randomIds = new Set();
      while (randomIds.size < 5) {
        randomIds.add(Math.floor(Math.random() * (151 - 1 + 1)) + 1);
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
      if (this.pokemonContador === 5) {
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
    },
    avanzarCard() {
      // Incrementar el índice para mostrar la siguiente tarjeta
      if (this.tarjetaActual < this.pokemons.length - 1) {
        this.tarjetaActual++;
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
