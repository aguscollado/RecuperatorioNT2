<template>
  <div id="app" class="container-fluid mt-3">
    <div class="jumbotron">
      <h1>SEGUNDO PARCIAL RECUPERATORIO</h1>
      <br />
      <gastos />
    </div>
  </div>
</template>

<script>
import gastos from "./componentes/Gastos.vue";
export default {
  name: "App",
  components: {
    gastos,
  },
};
</script>

<style>
#app {
}

h1 {
  color: blue;
}
</style>
