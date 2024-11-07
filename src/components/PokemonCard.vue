<!-- <template>
  <div class="card">
    <div class="card__content-img">
      <img
        :src="pokemon.sprites?.front_default"
        :alt="pokemon?.name"
        class="card-img"
        :style="{ filter: adivinado ? 'none' : ' brightness(0)' }"
      />
    </div>
    <h3 v-if="adivinado">{{ pokemon.name }}</h3>
    <div
      class="card-content"
      :style="{ display: adivinado ? 'none' : 'block' }"
    >
      <input
        type="text"
        placeholder="Escriba el nombre"
        class="card-input"
        v-model="pokemonInput"
        @keyup.enter="descubrir"
      />
      <button class="card-button" @click="descubrir">Descubrir</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "PokemonCard",
  props: {
    pokemon: Object,
  },
  data() {
    return {
      adivinado: false,
      pokemonInput: "",
    };
  },
  methods: {
    descubrir() {
      if (this.pokemonInput.toLowerCase() === this.pokemon.name) {
        this.adivinado = true;
        this.$emit("pokemonAdivinado", this.adivinado);
      }
    },
  },
};
</script>

<style scoped>
.card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 300px;
  border: 1px solid #ccc;
  border-radius: 5px;
  overflow: cover;
  margin: 10px;
  background-color: white;
}
.card__content-img {
  background-image: url("../assets/quien.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
  position: relative;
}
.card-img {
  width: 150px;
  height: auto;
}

.card-content {
  padding: 10px;
}

.card-input {
  width: 80%;
  padding: 5px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.card-button {
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style> -->
<!-- <template>
  <div class="card">
    <div class="card__content-img">
      <img
        :src="pokemon.sprites?.front_default"
        :alt="pokemon?.name"
        class="card-img"
        :style="{ filter: adivinado ? 'none' : 'brightness(0)' }"
      />
      <div v-if="error" class="error-overlay">Nombre Incorrecto</div>
    </div>
    <h3 v-if="adivinado">{{ pokemon.name }}</h3>
    <div
      class="card-content"
      :style="{ display: adivinado ? 'none' : 'block' }"
    >
      <input
        type="text"
        placeholder="Escriba el nombre"
        class="card-input"
        v-model="pokemonInput"
        @keyup.enter="descubrir"
      />
      <button class="card-button" @click="descubrir">Descubrir</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "PokemonCard",
  props: {
    pokemon: Object,
  },
  data() {
    return {
      adivinado: false,
      pokemonInput: "",
      error: false, // Propiedad para manejar el estado del error
    };
  },
  methods: {
    descubrir() {
      if (this.pokemonInput.toLowerCase() === this.pokemon.name.toLowerCase()) {
        this.adivinado = true;
        this.error = false; // Si es correcto, ocultar el mensaje de error
        this.$emit("pokemonAdivinado", this.adivinado);
      } else {
        this.error = true; // Mostrar el mensaje de error si es incorrecto
        setTimeout(() => {
          this.error = false; // Ocultar el mensaje de error después de 3 segundos
        }, 3000);
      }
    },
  },
};
</script>

<style scoped>
.card {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 300px;
  border: 1px solid #ccc;
  border-radius: 5px;
  overflow: hidden;
  margin: 10px;
  background-color: white;
}

.card__content-img {
  background-image: url("../assets/quien.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
  position: relative;
}

.card-img {
  width: 150px;
  height: auto;
}

.error-overlay {
  position: absolute;
  top: 50%;
  left: 40%;
  transform: translate(-50%, -50%);
  color: red;
  font-size: 25px;
  font-weight: bold;
 

}

.card-content {
  padding: 10px;
}

.card-input {
  width: 80%;
  padding: 5px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.card-button {
  padding: 5px 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
</style> -->
<template>
  <div class="card">
    <div class="card__content-img">
      <img
        :src="pokemon.sprites?.front_default"
        :alt="pokemon?.name"
        class="card-img"
        :style="{ filter: mostrado ? 'none' : 'brightness(0)' }"
      />
      <div v-if="error" class="error-overlay">Nombre Incorrecto</div>
    </div>
    <h3 v-if="mostrado" class="pokemon-nombre">{{ pokemon.name }}</h3>
    <div class="card-content" :style="{ display: mostrado ? 'none' : 'block' }">
      <input
        type="text"
        placeholder="Escriba el nombre"
        class="card-input"
        v-model="pokemonInput"
        @keyup.enter="descubrir"
      />
      <div class="content-button">
        <button class="card-button" @click="descubrir">Descubrir</button>
        <button class="card-button omitir-button" @click="omitir">
          Omitir
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "PokemonCard",
  props: {
    pokemon: Object,
  },
  data() {
    return {
      adivinado: false,
      mostrado: false, 
      pokemonInput: "",
      error: false, 
    };
  },
  methods: {
    descubrir() {
      if (this.pokemonInput.toLowerCase() === this.pokemon.name.toLowerCase()) {
        this.adivinado = true;
        this.mostrado = true;
        this.error = false;
        this.$emit("pokemonAdivinado", this.adivinado);
      } else {
        this.error = true;
        setTimeout(() => {
          this.error = false;
        }, 3000);
      }
    },
    omitir() {
      this.mostrado = true; 
      this.adivinado = false; 
    },
  },
};
</script>

<style scoped>
.card {
  padding: 5px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 300px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin: 10px;
  background-color: #ff0000;
}

.card__content-img {
  background-image: url("../assets/quien.png");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
  position: relative;
}

.card-img {
  width: 150px;
  height: auto;
}

.error-overlay {
  text-align: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: #ff0000;
  font-size: 40px;
  font-weight: bold;
}

.card-content {
  padding: 0; 
  margin: 0; 
}

.card-input {
  width: 100%;
  margin-top: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  height: 30px;
}

.card-button {
  background-color: #ffde00;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.omitir-button {
  background-color: #6c757d;
}
.content-button {
  display: flex;
  gap: 10px;
  padding: 0;
  margin: 0;
  justify-content: space-between; 
}
.pokemon-nombre {
  color: white;
  font-size: 30px;
  font-weight: bold;
  text-align: center;
}
</style>
