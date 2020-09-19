<template>
  <div id="app">
    <Display />
    <Search />
  </div>
</template>

<script>
import Display from "./components/Display";
import Search from "./components/Search";
require("dotenv").config();

export default {
  name: "App",
  components: {
    Display,
    Search
  },
  methods: {
    async getWeatherByCity(city) {
      city = city.trim() === "" ? "Mumbai" : city;
      const server = "api.openweathermap.org";
      const key = "3c0d3ab8f66567616f37a8dc9a672b8a";
      const url = `https://${server}/data/2.5/weather?q=${city}&APPID=${key}`;
      const response = await fetch(url, { mode: "cors" });
      return await response.json();
    },
    async getWeatherByCoords(lat, lon) {
      const server = "api.openweathermap.org";
      const key = "3c0d3ab8f66567616f37a8dc9a672b8a";
      const url = `https://${server}/data/2.5/weather?lat=${lat}&lon=${lon}&APPID=${key}`;

      const response = await fetch(url, { mode: "cors" });
      const weatherData = await response.json();

      return weatherData;
    }
  }
};
</script>

<style></style>
