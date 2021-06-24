<template>
  <img class="logo" alt="Reaction Timer logo" src="./assets/clock.png">
  <h1>Reaction Timer</h1>
  <Details />
</template>

<script>
import Details from './components/Details.vue'

export default {
  name: 'App',
  components: {
    Details
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.logo {
  width: 100%;
  max-width: 100px;
}
</style>
