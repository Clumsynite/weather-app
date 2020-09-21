<template>
  <div id="weather-card">
    <div id="content">
      <div id="name" :key="weather">
        {{ weather.name }}, {{ weather.sys.country }}
      </div>
      <div id="weather">
        {{ weather.weather }}
      </div>
      <div id="temperature">
        {{ weather }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Display",
  props: {
    weather: {
      type: Object
    }
  },
  data() {
    return {};
  },
  methods: {}
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
  font-size: 20px;
  color: white;
  text-shadow: 1px 2px 10px black;
}
</style>
