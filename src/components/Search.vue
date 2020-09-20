<template>
  <div id="get">
    <div id="search">
      <input type="text" v-model="city" />
      <button v-on:click="searchCity" id="search-btn" class="btn">
        <i class="material-icons">search</i>
      </button>
    </div>
    <div>
      <button v-on:click="getUserLocation" id="locate-btn" class="btn">
        <i class="material-icons">location_on</i>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Search",
  data() {
    return {
      userLat: "",
      userLon: "",
      city: ""
    };
  },
  created() {
    this.getUserLocation();
  },
  methods: {
    searchCity() {
      this.$emit("search-city", this.city);
      this.city = "";
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

      this.$emit("location-clicked", { lat, lon });
    }
  }
};
</script>

<style scoped>
#get {
  margin-top: 4vh;
  padding: 20px 20px 0 20px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  background-image: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5));
  width: 90vw;
  height: 90vh;
  border-radius: 15px;
}
#locate-btn {
  background: none;
  border: none;
}
</style>
