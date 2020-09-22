<template>
  <div id="weather-card">
    <div id="content" v-if="weather">
      <div id="date">
        {{ date }}
      </div>
      <div id="location">{{ city }}, {{ country }}</div>
      <div id="weather">
        <div id="weather-row">
          <div id="weather-name">Weather: {{ weatherName }}</div>
          <div id="weather-like">Description: {{ weatherDesc }}</div>
        </div>
        <div id="icon-div">
          <img :src="icon" alt="" id="weather-icon" />
        </div>
      </div>
      <div
        id="temperature-div"
        @click="toggleUnit"
        title="Click to toggle Standard Unit"
      >
        Temperature:
        {{ unit === "c" ? `${celsius}&deg;C` : `${fahrenheit}&deg;F` }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Display",
  data() {
    return {
      unit: "c",
      date: this.formatDate(new Date())
    };
  },
  props: {
    weather: {
      type: Object
    }
  },
  created() {
    this.startInterval();
  },
  computed: {
    celsius: function() {
      return (this.temperature - 273.15).toFixed(2);
    },
    fahrenheit: function() {
      return ((this.temperature * 9) / 5 - 459.67).toFixed(2);
    },
    city: function() {
      return this.weather.name;
    },
    country: function() {
      return this.weather.sys.country;
    },
    temperature: function() {
      return this.weather.main.temp;
    },
    icon: function() {
      return this.weather.icon;
    },
    weatherName: function() {
      return this.weather.weather[0].main;
    },
    weatherDesc: function() {
      return this.weather.weather[0].description;
    },
    feelsLike: function() {
      return this.weather.main.feels_like;
    }
  },
  methods: {
    toggleUnit() {
      this.unit = this.unit === "c" ? "f" : "c";
    },
    startInterval() {
      setInterval(() => {
        this.date = this.formatDate(new Date());
      }, 1000);
    },
    formatDate(date) {
      let hours = date.getHours();
      let minutes = date.getMinutes();
      let ampm = hours >= 12 ? "pm" : "am";
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
      hours = hours % 12;
      hours = hours ? hours : 12;
      minutes = minutes < 10 ? "0" + minutes : minutes;
      let strTime = `${hours}:${minutes}:${date.getSeconds()} ${ampm}`;
      return `${strTime}, ${months[date.getMonth()]} ${date.getDate()}`;
    }
  }
};
</script>

<style scoped>
#weather-card {
  width: 90vw;
  height: 80vh;
  border-radius: 0 0 15px 15px;
  border: 2px solid black;
  border-top: none;
  background-image: linear-gradient(rgba(0, 0, 0, 0.3), rgba(0, 0, 0, 0.3));
}

#content {
  padding: 10px;
  color: white;
  text-shadow: 1px 2px 10px black;
}
#location {
  font-size: 30px;
  font-weight: bold;
}
#date {
  font-size: 16px;
  color: black;
  text-shadow: 1px 1px 20px white;
  font-weight: bold;
}
#weather {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  font-size: 20px;
}
#weather-row {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}
#icon-div img {
  transform: scale(1.5);
}
#temperature-div {
  user-select: none;
  font-size: 25px;
}
#temperature-div:hover {
  cursor: pointer;
}
</style>
