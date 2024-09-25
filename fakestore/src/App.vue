<template>
  <div id="app">
    <ProductGrid msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import ProductGrid from './components/ProductGrid.vue'

export default {
  components: {
    ProductGrid
  }
}
</script>

<style>
h1, h2, h3, h4, h5 {
  font-family: 'Playfair Display', serif;
}

/* Estilo de fondo con imagen y degradado */
body {
  background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('D:/fake store/fakestore/src/assets/fondo2.png');
  background-size: cover;
  background-position: center;
  font-family: 'Roboto', sans-serif;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
