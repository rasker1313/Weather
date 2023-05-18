<template>
  <div>
    <div>
      <p>Введіть назву міста англійською</p>
      <p><input v-model="city" @keyup.enter="getCoordinates"/></p>
      <p><button @click="getCoordinates">Пошук</button></p>
      <div v-if="!isCityEmpty">
        <button @click="changeLimit(4)" :disabled="limit==4">Погода на пів дня</button>
        <button @click="changeLimit(8)" :disabled="limit==8">Погода на день</button>
        <button @click="changeLimit(24)" :disabled="limit==24">Погода на 3 дні</button>
      </div>
    </div>
    <div v-if="isLoad">
      <div><p>Погода в місті {{ city }}</p></div>
<!-- <div><p>З координатами: lat: {{ coordinates[0] }}, lon: {{ coordinates[1] }}</p></div>-->
      <hr>
      <div v-if="weather && isLoad" class="weather-container">
        <div class="weather-block" v-for="(day, key) in slicedDays()" :key="key">
            <div><img :src="imageUrl(day.weather[0].icon)" alt="sun"></div>
            <p>Температура: {{day.main.temp}}</p>
            <p>Вітер: {{day.wind.speed}}</p>
            <p>Хмарність: {{day.clouds.all}}</p>
            <p>Атмосферний тиск: {{day.main.pressure}}</p>
            <p>Час: {{ editDate(day.dt_txt, 5, -3) }}</p>
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
      isCityEmpty: true,
      weather: {},
      isLoad: false,
      coordinates: null,
      error: null,
      limit: 4,
    }
  },
  methods: {
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
        this.isCityEmpty = false;
      } catch (error){
        this.error = error;
        this.isLoad = true;
      }
    },
    changeLimit(lim){
      this.limit = lim;
      this.get3DaysWeatherData();
    },
    slicedDays(){
      return Object.fromEntries(Object.entries(this.weather.list).slice(0, this.limit));
    },
    editDate(date, start, end){
      return date.slice(start, end)
    },
    imageUrl(iconName){
      console.log(iconName)
      return `/img/${iconName}.png`;
    }
  },
}
</script>

<style scoped>

  .disable{
    opacity:0.5;
    user-select: none;
  }
  .weather-container{
    display:flex;
    justify-content: space-between;
    flex-wrap: wrap;
  }
  .weather-block{
    width:25%;
  }
</style>