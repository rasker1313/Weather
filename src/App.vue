<template>
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
html{
  height:100%;
  background: rgb(64,148,255);
  background: linear-gradient(0deg, rgba(64,148,255,1) 0%, rgba(255,255,255,1) 50%, rgba(251,250,124,1) 100%);
  background-attachment: fixed;
}
</style>
