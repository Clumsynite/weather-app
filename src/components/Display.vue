<template>
  <div id="weather-card">
    <div id="content" v-if="weather">
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
      <div id="temperature-div" @click="toggleUnit">
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
      unit: "c"
    };
  },
  props: {
    weather: {
      type: Object
    }
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
    }
  },
  methods: {
    toggleUnit() {
      this.unit === "c" ? "f" : "c";
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
  font-size: 28px;
  font-weight: bold;
}
#weather {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
#weather-row {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}
#temperature-div {
  user-select: none;
}
#temperature-div:hover {
  cursor: pointer;
}
</style>
