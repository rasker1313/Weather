<template>
  <div>
    <div>
      <p>Введіть назву міста англійською</p>
      <p><input v-model="city"></p>
      <p><input type="button" @click="getWeatherData" value="Пошук"></p>
    </div>
    <div v-if="isLoad">
      <div>Погода в місті {{ city }}</div>
      <div v-if="weather && isLoad">
        <div><img :src="imageUrl" alt="sun"></div>
        <p>Температура: {{weather.main.temp}}</p>
        <p>Вітер: {{weather.wind.speed}}</p>
        <p>Хмарність: {{weather.clouds.all}}</p>
        <p>Атмосферний тиск: {{weather.main.pressure}}</p>
      </div>
      <div v-if="error">{{ error }}</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "WeatherMain",
  data() {
    return {
      city: '',
      weather: {},
      isLoad: false,
      error: null
    }
  },
  /*mounted(){
    this.getWeatherData()
  },*/
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
        this.isLoad = true;
      } catch (error){
        this.error = error;
        this.isLoad = true;
      }
    },
  },
  computed:{
    imageUrl(){
      return `/img/${this.weather.weather[0].icon}.png`;
    }
  }
}
</script>

<style scoped>

</style>