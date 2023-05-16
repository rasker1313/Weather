<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <WeatherMain />
  <hr/>
  <WeatherFor3Days />
</template>

<script>
import WeatherMain from './components/WeatherMain.vue'
import WeatherFor3Days from "@/components/WeatherFor3Days";

export default {
  name: 'App',
  components: {
    WeatherFor3Days,
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
