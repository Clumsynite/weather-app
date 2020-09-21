<template>
  <div id="app">
    <img
      id="background-img"
      v-bind:src="gif !== '' ? gif : whereAreYou"
      alt=""
    />
    <div id="weather-div">
      <Search
        @search-city="getWeatherByCity($event)"
        @location-clicked="getWeatherByCoords($event)"
      />
      <Display v-if="load" v-bind:weather="currentWeather" />
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
      gif: "",
      whereAreYou:
        "https://media1.tenor.com/images/9aade0fc6248deb37fd529cdf39ec7ab/tenor.gif?itemid=12458697",
      load: false
    };
  },
  methods: {
    async getWeatherByCity(city) {
      city = city.trim() === "" ? "Mumbai" : city;
      const url = `q=${city}`;
      this.getWeather(url);
    },
    async getWeatherByCoords(coords) {
      const url = `lat=${coords.lat}&lon=${coords.lon}`;
      this.getWeather(url);
    },
    async getWeather(query) {
      try {
        const server = "api.openweathermap.org";
        const key = "3c0d3ab8f66567616f37a8dc9a672b8a";
        const url = `https://${server}/data/2.5/weather?${query}&APPID=${key}`;
        const response = await fetch(url, { mode: "cors" });
        if (response.status === 404) {
          alert("city not found");
          return;
        }
        const weather = await response.json();
        this.weatherCondition = weather.weather[0].main;
        this.currentWeather = weather;
        const iconUrl = `http://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`;
        this.currentWeather.icon = iconUrl;
        this.load = true;
        this.getGif(this.weatherCondition);
      } catch (e) {
        console.log(e.message);
      }
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
@import url("https://fonts.googleapis.com/css2?family=Quicksand&display=swap");
* {
  margin: 0;
  padding: 0;
}
body {
  margin: 0;
  padding: 0;
  overflow-y: hidden;
  font-family: "Quicksand", sans-serif;
}
#background-img {
  position: absolute;
  width: 100vw;
  height: 100vh;
  z-index: -1;
}
#weather-div {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>
