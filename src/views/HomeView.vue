<template>
  <h2 v-if="juegoIniciado">
    Pokémon descubiertos: <span class="contador">{{ contador }}</span>
  </h2>
  <main>
    <div v-if="!juegoIniciado" class="text-center content-but-sel">
      <label for="generationSelect" class="form-label"
        >Selecciona una Generación:</label
      >
      <select
        id="generationSelect"
        class="form-select mb-3"
        v-model="selectedGeneration"
      >
        <option value="all">Todos</option>
        <option value="gen1">Generación 1</option>
        <option value="gen2">Generación 2</option>
        <option value="gen3">Generación 3</option>
        <option value="gen4">Generación 4</option>
        <option value="gen5">Generación 5</option>
        <option value="gen6">Generación 6</option>
        <option value="gen7">Generación 7</option>
      </select>
      <label for="dificultad" class="form-label">Selecciona Dificultad:</label>
      <select id="dificultad" class="form-select mb-3" v-model="dificultad">
        <option value="facil">Fácil</option>
        <option value="medio">Medio</option>
        <option value="dificil">Difícil</option>
      </select>
      <button @click="iniciarJuego" class="btn btn-inicio">
        Iniciar Juego
      </button>
    </div>
    <div v-if="juegoIniciado" class="tarjetas-contenedor">
      <div v-for="(pokemon, index) in pokemons" :key="pokemon.id">
        <PokemonCard
          v-show="index === tarjetaActual"
          :pokemon="pokemon"
          @pokemonAdivinado="sumaContador"
          @pokemonOmitido="omitirPokemon"
          @avanzarCard="avanzarCard"
        />
      </div>
    </div>
    <ModalResultado
      v-if="modalVisible"
      :pokemonadivinados="pokemonadivinados"
      :pokemonOmitidos="pokemonOmitidos"
      @cerrarModal="cerrarModal"
    />
    <stars />
    <AnimacionPikachu />
    <AnimacionPokeball />
  </main>
</template>

<script>
import PokemonCard from "../components/PokemonCard.vue";
import ModalResultado from "../components/ModalResultado.vue";
import Stars from "@/components/Stars.vue";
import AnimacionPikachu from "@/components/AnimacionPikachu.vue";
import AnimacionPokeball from '@/components/AnimacionPokeball.vue';
import axios from "axios";

export default {
  components: {
    PokemonCard,
    ModalResultado,
    Stars,
    AnimacionPikachu,
    AnimacionPokeball,
  },
  data() {
    return {
      pokemons: [],
      pokemonadivinados: [],
      pokemonOmitidos: [],
      juegoIniciado: false,
      modalVisible: false,
      pokemonContador: 0,
      tarjetaActual: 0,
      selectedGeneration: "all",
      dificultad: "facil",
      rondasPorAdivinar: 5,
    };
  },
  methods: {
    async iniciarJuego() {
      this.juegoIniciado = true;
      this.pokemonadivinados = [];
      this.pokemonOmitidos = [];
      this.pokemonContador = 0;
      this.tarjetaActual = 0;
      this.modalVisible = false;
      this.asignarRondasPorDificultad();

      let startId = 1;
      let endId = 151;

      switch (this.selectedGeneration) {
        case "gen1":
          startId = 1;
          endId = 151;
          break;
        case "gen2":
          startId = 152;
          endId = 251;
          break;
        case "gen3":
          startId = 252;
          endId = 386;
          break;
        case "gen4":
          startId = 387;
          endId = 493;
          break;
        case "gen5":
          startId = 494;
          endId = 649;
          break;
        case "gen6":
          startId = 650;
          endId = 721;
          break;
        case "gen7":
          startId = 722;
          endId = 809;
          break;
        default:
          startId = 1;
          endId = 809;
      }

      const randomIds = new Set();
      while (randomIds.size < this.rondasPorAdivinar) {
        randomIds.add(
          Math.floor(Math.random() * (endId - startId + 1)) + startId
        );
      }

      const idsArray = Array.from(randomIds);
      const pokemonData = await axios.all(
        idsArray.map((id) =>
          axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
        )
      );

      this.pokemons = pokemonData.map((pokemon) => pokemon.data);
    },
    asignarRondasPorDificultad() {
      if (this.dificultad === "facil") {
        this.rondasPorAdivinar = 5;
      } else if (this.dificultad === "medio") {
        this.rondasPorAdivinar = 10;
      } else if (this.dificultad === "dificil") {
        this.rondasPorAdivinar = 20;
      }
    },
    async sumaContador(adivinado) {
  if (adivinado) {
    this.pokemonadivinados.push(adivinado);
    this.pokemonContador++;
   
  }
  this.checkRondaTerminada();
},


    omitirPokemon(pokemon) {
      this.pokemonOmitidos.push(pokemon);
      this.pokemonContador++;
      this.checkRondaTerminada();
    },
    checkRondaTerminada() {
      if (this.pokemonContador === this.rondasPorAdivinar) {
        this.modalVisible = true;
      }
    },
    cerrarModal() {
      this.juegoIniciado = false;
      this.pokemons = [];
      this.pokemonadivinados = [];
      this.pokemonOmitidos = [];
      this.pokemonContador = 0;
      this.modalVisible = false;
    },
    avanzarCard() {
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
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}
label {
  font-size: 20px;
  font-weight: bold;
  color: white;
  margin-top: 40px;
}
.content-but-sel {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.btn-inicio {
  margin: 20px auto;
  padding: 10px 20px;
  font-size: 18px;
  cursor: pointer;
  background: linear-gradient(135deg, #ffcb05, #3b4cca);
  border: none;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  color: white;
  transition: background 0.3s, transform 0.2s;
}

.btn-inicio:hover {
  background: linear-gradient(135deg, #ffd700, #27408b);
  transform: translateY(-3px);
}

select {
  padding: 8px;
  font-size: 16px;
  border-radius: 8px;
  border: 2px solid #3b4cca;
  background-color: #f8f9fa;
  color: #3b4cca;
  transition: border-color 0.3s;
}

select:focus {
  border-color: #ffcb05;
  outline: none;
}

.tarjetas-contenedor {
  display: flex;
  justify-content: center;
  height: 80vh; /* Ajusta el alto de la vista para que la tarjeta esté centrada */
  width: 100%;
  position: relative;
}

.pokemon-card {
  width: 200px; /* Ajusta el tamaño de la tarjeta */
  height: 300px; /* Ajusta el tamaño de la tarjeta */
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

</style>
