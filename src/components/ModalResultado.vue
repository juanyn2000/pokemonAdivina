<template>
    <div class="modal-overlay">
      <div class="modal">
        <h2>Resultados de la ronda</h2>
        <p>Pokémon adivinados: {{ pokemonadivinados.length }}</p>
        <p>Pokémon omitidos: {{ pokemonOmitidos.length }}</p>
        <button @click="cerrarModal">CERRAR</button>
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
        this.$emit("cerrarModal");
      },
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
  }
  
  button {
    width: 80%;
    height: 50px;
    background-color: #b3a125;
    color: white;
    font-weight: bold;
    padding: 5px;
    border: 1px solid white;
    border-radius: 5px ;
    cursor: pointer;
    font-size: 20px;
  }
  button:hover {
    background-color: #ffe100;
  }
  </style>
  