<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="contenedor-img">
    <img
      class="img-titulo"
      src="./assets/pokemonTitulo.png"
      alt="titulo-pokemon"
    />
  </div>
  <h2>¿Quien es ese Pokémon?</h2>

  <RouterView />
</template>

<style scoped>
/* Asegúrate de que el contenedor ocupe todo el ancho y alto de la pantalla */


.contenedor-img {
  display: flex;
  justify-content: center;
}
.img-titulo{
  width: 500px;
  height: auto;
}

h2 {
  font-size: 1.5em;
  color: white;
  text-align: center;
  font-size: 50px;
  margin: 0%;
}

</style>
