<template>
  <div>
    <input v-model="city" placeholder="Enter a city name" />
    <button @click="getCoordinates">Get Coordinates</button>
    <div v-if="coordinates">
      <p>Latitude: {{ coordinates[0] }}</p>
      <p>Longitude: {{ coordinates[1] }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      coordinates: null,
    };
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
            } else {
              console.error('No results found');
            }
          })
          .catch(error => {
            console.error(error);
          });
    },
  },
};
</script>

<style scoped>

</style>