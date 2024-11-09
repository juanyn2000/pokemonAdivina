<template>
  <div class="modal-overlay">
    <div class="modal">
      <h2>Resultados de la ronda</h2>
      <p>Pokémon adivinados: {{ pokemonadivinados.length }}</p>
      <p>Pokémon omitidos: {{ pokemonOmitidos.length }}</p>
      <button ref="closeButton" @click="cerrarModal">CERRAR</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pokemonadivinados: Array,
    pokemonOmitidos: Array,
  },
  methods: {
    cerrarModal() {
      const overlay = this.$el;
      overlay.classList.remove("show");
      setTimeout(() => {
        this.$emit("cerrarModal");
      }, 300); // Tiempo igual al de la transición CSS
    },
  },
  mounted() {
    // Añade la clase para mostrar el modal con la transición
    this.$nextTick(() => {
      const overlay = this.$el;
      overlay.classList.add("show");

      // Asegurarse de que el enfoque ocurra después de la transición
      setTimeout(() => {
        this.$refs.closeButton.focus();
      }, 300); // Tiempo igual al de la transición CSS
    });
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s ease-in-out, visibility 0.3s ease-in-out;
}

.modal-overlay.show {
  opacity: 1;
  visibility: visible;
}

.modal {
  background-color: #ff0000;
  color: white;
  font-weight: bold;
  font-size: 20px;
  padding: 20px;
  border-radius: 10px;
  width: 300px;
  text-align: center;
  border: 2px solid white;
  transform: scale(0.8);
  transition: transform 0.3s ease-in-out;
}

.modal-overlay.show .modal {
  transform: scale(1);
}

button {
  width: 80%;
  height: 50px;
  background-color: #b3a125;
  color: white;
  font-weight: bold;
  padding: 5px;
  border: 1px solid white;
  border-radius: 5px;
  cursor: pointer;
  font-size: 20px;
}
button:hover {
  background-color: #ffe100;
}
</style>
