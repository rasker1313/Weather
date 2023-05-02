<template>
  <div>Погода в місті {{ city }}</div>
  <div v-if="isLoading">Завантажується...</div>
  <div v-if="weather && !isLoading">
    <p>Температура: {{weather.main.temp}}</p>
    <p>Вітер: {{weather.wind.speed}}</p>
    <p>Хмарність: {{weather.clouds.all}}</p>
    <p>Атмосферний тиск: {{weather.main.pressure}}</p>
  </div>
  <div v-if="error">{{ error }}</div>
</template>

<script>
import axios from 'axios';

export default {
  name: "WeatherMain",
  data() {
    return {
      city: 'Lviv',
      weather: {},
      isLoading: true,
      error: null
    }
  },
  mounted(){
    this.getWeatherData()
  },
  methods: {
    async getWeatherData(){
      try {
        const response = await axios.get('https://api.openweathermap.org/data/2.5/weather', {
          params: {
            q:this.city,
            units: 'metric',
            appid: '378b8a9e7ac3ae847715632a7fde0254'
          }
        })
        this.weather = response.data;
        this.isLoading = false;
      } catch (error){
        this.error = error;
        this.isLoading = false;
      }
    }
  }
}
</script>

<style scoped>

</style>