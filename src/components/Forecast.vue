<template>
  <transition-group
    id="forecast-div"
    enter-active-class="animate__animated animate__fadeIn animate__slow"
    title="09:00 am Forecast"
  >
    <div class="cell" v-for="day in forecast" :key="day.main.temp">
      <div class="cell-day">
        {{ formatDate(day.dt_txt) }}
      </div>
      <div class="cell-icon">
        <img :src="getWeatherIcon(day)" alt="" class="weather-icon" />
      </div>
      <div class="cell-weather">
        {{ getWeather(day) }}
      </div>
      <div class="cell-temp" @click="toggleUnit" title="CLick here to toggle Standard Unit">
        {{
          unit === "c"
            ? `${getTemperature(day)}&deg;C`
            : `${getTemperature(day)}&deg;F`
        }}
      </div>
    </div>
  </transition-group>
</template>

<script>
export default {
  name: "Forecast",
  data() {
    return {
      forecastAll: [],
      unit: "c"
    };
  },
  props: {
    coord: {
      type: Object
    }
  },
  mounted() {
    this.getForecast(this.coord.lat, this.coord.lon);
  },
  methods: {
    async getForecast(lat, lon) {
      try {
        const server = "api.openweathermap.org";
        const key = "3c0d3ab8f66567616f37a8dc9a672b8a";
        const url = `https://${server}/data/2.5/forecast?lat=${lat}&lon=${lon}&APPID=${key}`;
        const response = await fetch(url, { mode: "cors" });
        if (response.status === 404) {
          alert("city not found");
          return;
        }
        const weather = await response.json();
        this.forecastAll = weather.list;
      } catch (e) {
        console.log(e.message);
      }
    },
    formatDate(date) {
      let string = date.split(" ")[0];
      let day = string.split("-")[2];
      let months = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec"
      ];
      let month = months[Number(string.split("-")[1] - 1)];
      return `${day} ${month}`;
    },
    getWeather: function(day) {
      return day.weather[0].main;
    },
    getTemperature: function(day) {
      const temp = day.main.temp;
      return this.unit === "c" ? this.celsius(temp) : this.fahrenheit(temp);
    },
    toggleUnit() {
      this.unit = this.unit === "c" ? "f" : "c";
    },
    celsius: function(value) {
      return (value - 273.15).toFixed(2);
    },
    fahrenheit: function(value) {
      return ((value * 9) / 5 - 459.67).toFixed(2);
    },
    getWeatherIcon: function(code) {
      const iconUrl = `http://openweathermap.org/img/wn/${code.weather[0].icon}@2x.png`;
      return iconUrl;
    }
  },
  computed: {
    forecast: function() {
      return this.forecastAll.filter((val, i) => {
        return (i + 1) % 8 === 0;
      });
    }
  }
};
</script>

<style scoped>
#forecast-div {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  flex-wrap: wrap;
  margin-top: 40px;
  padding: 5px;
  background-image: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3));
}
.cell {
  display: flex;
  flex-direction: column;
}
.cell-temp {
  user-select: none;
  cursor: pointer;
}
.cell-icon {
  object-fit: contain;
}
@media screen and (max-width: 600px) {
  .weather-icon {
    height: 15vw;
    width: 15vw;
  }
}
@media screen and (max-width: 360px) {
  #forecast-div {
    flex-wrap: wrap;
  }
}
</style>
