<template>
  <div>
    <input type="text" v-model="city" />
    <button v-on:click="searchCity">Search</button>
    <button v-on:click="getUserLocation">Location at your Place</button>
  </div>
</template>

<script>
import App from "../App";
export default {
  name: "Search",
  data() {
    return {
      userLat: "",
      userLon: "",
      city: ""
    };
  },
  methods: {
    async searchCity() {
      const weather = await App.methods.getWeatherByCity(this.city);

      console.log(weather);
    },
    getUserLocation() {
      navigator.geolocation.getCurrentPosition(this.locationSuccess, () => {
        console.log("Unabe to retrieve your Location");
      });
    },
    async locationSuccess(position) {
      const lat = position.coords.latitude;
      const lon = position.coords.longitude;

      this.userLat = lat;
      this.userLon = lon;

      const data = await App.methods.getWeatherByCoords(lat, lon);
      console.log(data);
    }
  }
};
</script>

<style scoped></style>
