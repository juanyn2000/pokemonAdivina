<template>
  <div class="card">
    <div class="card__content-img">
      <img
        ref="cardImage"
        :src="pokemon.sprites?.front_default"
        :alt="pokemon?.name"
        class="card-img"
        :style="{ filter: mostrado ? 'none' : 'brightness(0)' }"
        :class="{ 'brillo-descubierto': adivinado }"
      />
      <div v-if="error" class="error-overlay">
        <img class="img-error" src="../assets/error.png" alt="cruz indica error" />
      </div>
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
        <button class="button-descubrir" @click="descubrir">Descubrir</button>
        <button class="button-omitir" @click="omitir">Omitir</button>
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
        // Activar la animación de brillo
        this.$nextTick(() => {
          this.$refs.cardImage.classList.add('brillo-descubierto');
          setTimeout(() => {
            this.$refs.cardImage.classList.remove('brillo-descubierto');
          }, 1500); // Duración de la animación de brillo
        });
        this.$emit("pokemonAdivinado", this.adivinado);
      } else {
        this.error = true;
        this.$nextTick(() => {
          const errorOverlay = document.querySelector('.error-overlay');
          if (errorOverlay) {
            errorOverlay.classList.remove('animation-reset');
            void errorOverlay.offsetWidth; 
            errorOverlay.classList.add('animation-reset');
          }
        });
        setTimeout(() => {
          this.error = false;
        }, 3000);
      }
    },
    omitir() {
      this.mostrado = true;
      this.adivinado = false;
      // Emitir el evento para Pokémon omitido
      this.$emit("pokemonOmitido", this.pokemon);
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

.brillo-descubierto {
  animation: animacionBrillo 1.5s ease-in-out;
}

@keyframes animacionBrillo {
  0% {
    filter: brightness(1);
  }
  50% {
    filter: brightness(2.5);
  }
  100% {
    filter: brightness(1);
  }
}

.card-content {
  padding: 10px;
}

.card-input {
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 5px;
  height: 30px;
}
button{
  width: 100%;
  height: 40px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.button-descubrir {
  background-color: #b3a125;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
.button-descubrir:hover {
  background-color: #ffe100;
}

.button-omitir {
  background-color: #757575;
}
.button-omitir:hover {
  background-color: #a5a5a5;
}

.content-button {
  display: flex;
  gap: 10px;
  padding: 0;
  margin: 0;
}

.pokemon-nombre {
  color: white;
  font-size: 30px;
  font-weight: bold;
  text-align: center;
}

.error-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: aparecerDesaparecer 3s ease-in-out;
}

.img-error {
  width: 150px;
  height: 150px;
}

@keyframes aparecerDesaparecer {
  0% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.5);
  }
  10% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1.1);
  }
  90% {
    opacity: 1;
    transform: translate(-50%, -50%) scale(1);
  }
  100% {
    opacity: 0;
    transform: translate(-50%, -50%) scale(0.5);
  }
}
</style>
