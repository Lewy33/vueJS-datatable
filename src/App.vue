<template>
  <div id="app">
    <img src="./assets/logo.png">
    <Liste msg="Welcome to Your Vue.js App"/>

  </div>
</template>

<script>
import Liste from './components/Liste.vue'


export default {
  name: 'app',
  components: {
    Liste
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
