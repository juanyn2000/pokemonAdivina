<!-- <script>
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
</style> -->
<!-- <script>
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
    // Generar 20 IDs aleatorios únicos entre 1 y 898
    const randomIds = new Set();
    while (randomIds.size < 20) {
      randomIds.add(Math.floor(Math.random() * 151) + 1);
    }

    // Convertir el set a un array para iterar
    const idsArray = Array.from(randomIds);

    // Realizar las solicitudes para cada Pokémon usando los IDs aleatorios
    const pokemonData = await axios.all(
      idsArray.map((id) => axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`))
    );

    // Asignar los datos obtenidos al array pokemons
    this.pokemons = pokemonData.map((pokemon) => pokemon.data);
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
    <div v-for="pokemon in pokemons" :key="pokemon.id">
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
</style> -->
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
      juegoIniciado: false,
    };
  },
  methods: {
    async iniciarJuego() {
      this.juegoIniciado = true;

      // Genera ides aleatorios
      const randomIds = new Set();
      while (randomIds.size < 20) {
        randomIds.add(Math.floor(Math.random() * 151) + 1);
      }

      const idsArray = Array.from(randomIds);

      const pokemonData = await axios.all(
        idsArray.map((id) =>
          axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`)
        )
      );

      // Asigna  datos al array pokemons
      this.pokemons = pokemonData.map((pokemon) => pokemon.data);
    },
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
  <h2 v-if="juegoIniciado">Pokémon descubiertos: <span class="contador">{{ contador }} </span> </h2>
  <main>
    <button v-if="!juegoIniciado" @click="iniciarJuego">Iniciar Juego</button>

    <div v-if="juegoIniciado" v-for="pokemon in pokemons" :key="pokemon.id">
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
button {
  margin: 40px auto;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  background-color: rgb(195, 11, 11);
  color: white;
}
.contador{
  color: #ee830a;
  font-size: 40px;
  font-weight: bold;
}
</style>
