<template>
  <div id="app">
    <img id="background-img" v-bind:src="gif" alt="" />
    <div id="weather-card">
      <Search
        @search-city="getWeatherByCity($event)"
        @location-clicked="getWeatherByCoords($event)"
      />
      <Display />
    </div>
  </div>
</template>

<script>
import Display from "./components/Display";
import Search from "./components/Search";

export default {
  name: "App",
  components: {
    Display,
    Search
  },
  data() {
    return {
      currentWeather: {},
      weatherCondition: "",
      gif: ""
    };
  },
  methods: {
    async getWeatherByCity(city) {
      try {
        city = city.trim() === "" ? "Mumbai" : city;
        const server = "api.openweathermap.org";
        const key = "3c0d3ab8f66567616f37a8dc9a672b8a";
        const url = `https://${server}/data/2.5/weather?q=${city}&APPID=${key}`;
        const response = await fetch(url, { mode: "cors" });

        const weather = await response.json();
        this.weatherCondition = weather.weather[0].main;
        this.getGif(this.weatherCondition);
        console.log(response.status);
        console.log(weather);
      } catch (e) {
        console.log(e.message);
      }
    },
    async getWeatherByCoords(coords) {
      const server = "api.openweathermap.org";
      const key = "3c0d3ab8f66567616f37a8dc9a672b8a";
      const url = `https://${server}/data/2.5/weather?lat=${coords.lat}&lon=${coords.lon}&APPID=${key}`;

      const response = await fetch(url, { mode: "cors" });
      const weatherData = await response.json();

      console.log(weatherData);
      this.weatherCondition = weatherData.weather[0].main;
      this.getGif(this.weatherCondition);
    },
    async getGif(query) {
      const key = "aIYPP1yuQyj5KBugwlKzgXSHpybIf1dD";
      const url = `https://api.giphy.com/v1/gifs/translate?api_key=${key}&s=${query}`;
      const response = await fetch(url, { mode: "cors" });
      const gif = await response.json();
      if (query.toLowerCase() === "haze") {
        this.gif =
          "https://media2.giphy.com/media/dgeIH5RPynA6Q/giphy.gif?cid=ecf05e47537da5bxohc1alhzkdysjjomkrvi1775sxh5sl5x&rid=giphy.gif";
        return;
      }
      this.gif = gif.data.images.original.url;
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/icon?family=Material+Icons");

body {
  margin: 0;
  overflow-y: hidden;
}
#background-img {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: -1;
}
#weather-card {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
