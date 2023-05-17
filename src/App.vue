<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <WeatherMain />
</template>

<script>
import WeatherMain from './components/WeatherMain.vue'

export default {
  name: 'App',
  components: {
    WeatherMain
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
</style>
