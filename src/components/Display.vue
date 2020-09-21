<template>
  <div id="weather-card"></div>
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
  background-image: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5));
}
</style>
