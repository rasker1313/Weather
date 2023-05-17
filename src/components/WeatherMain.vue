<template>
  <div>
    <div>
      <p>Введіть назву міста англійською</p>
      <p><input v-model="city"/></p>
      <p><button @click="getCoordinates">Пошук</button></p>
<!--      <p><input type="button" @click="getWeatherData" value="Пошук"></p>-->
    </div>
    <div v-if="isLoad">
      <div><p>Погода в місті {{ city }}</p></div>
      <div><p>З координатами: lat: {{ coordinates[0] }}, lon: {{ coordinates[1] }}</p></div>
      <hr>
      <div v-if="weather && isLoad" >
        <div class="" v-for="(day, key) in this.weather.list" :key="key">
<!--          <div><img :src="imageUrl" alt="sun"></div>-->
          <p>Температура: {{day.main.temp}}</p>
          <p>Вітер: {{day.wind.speed}}</p>
          <p>Хмарність: {{day.clouds.all}}</p>
          <p>Атмосферний тиск: {{day.main.pressure}}</p>
          <p>Час: {{ day.dt_txt }}</p>
          <hr>
        </div>
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
      coordinates: null,
      error: null,
    }
  },
  methods: {
    /*async getWeatherData(){
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
    },*/
    getCoordinates() {
      axios
          .get('https://nominatim.openstreetmap.org/search', {
            params: {
              q: this.city,
              format: 'json',
            },
          })
          .then(response => {
            if (response.data.length > 0) {
              const { lat, lon } = response.data[0];
              this.coordinates = [lat, lon];
              this.get3DaysWeatherData();
            } else {
              console.error('No results found');
            }
          })
          .catch(error => {
            console.error(error);
          });
    },
    async get3DaysWeatherData(){
      try {
        const response = await axios.get('https://api.openweathermap.org/data/2.5/forecast', {
          params: {
            lat:this.coordinates[0],
            lon:this.coordinates[1],
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
    /*imageUrl(){
      return `/img/${this.weather.weather[0].icon}.png`;
    }*/
  }
}
</script>

<style scoped>

</style>