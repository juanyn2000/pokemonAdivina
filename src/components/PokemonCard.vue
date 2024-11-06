<template>
  <div class="card">
    <img
      :src="pokemon.sprites?.front_default"
      :alt="pokemon?.name"
      class="card-img"
      :style="{ filter: adivinado ? 'none' : ' brightness(0)' }"
    />
    <h3 v-if="adivinado">{{pokemon.name}}</h3>
    <div class="card-content" :style="{ display: adivinado ? 'none' : 'block'}">
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
        this.$emit("pokemonAdivinado",this.adivinado);
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
  background-color: rgba(255, 255, 255, 0.68);
}

.card-img {
  width: 70%;
  height: auto;
  object-fit: cover;
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
</style>
