<template>
  <div id="get">
    <form id="get-data">
      <input
        type="text"
        v-model="city"
        id="get-city"
        placeholder="Enter City name:"
        autocomplete="off"
      />
      <button v-on:click="searchCity" id="search-btn" class="btn" type="submit">
        <i class="large material-icons">search</i>
      </button>
      <button v-on:click="getUserLocation" id="locate-btn" class="btn">
        <i class="large material-icons">location_on</i>
      </button>
    </form>
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
      if (event) {
        event.preventDefault();
      }
      this.$emit("search-city", this.city);
      this.city = "";
    },
    getUserLocation() {
      navigator.geolocation.getCurrentPosition(this.locationSuccess, () => {
        alert("Unable to retrieve your Location");
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
  border-radius: 15px 15px 0 0;
  border: 2px solid black;
  border-bottom: none;
  width: 90vw;
  background-image: linear-gradient(rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0.2));
}
#get-data {
  padding: 10px;
  display: flex;
  flex-direction: row;
}
.btn {
  background-color: aliceblue;
  color: #111;
  border: 1px solid white;
  outline: none;
  padding: 5px;
  width: 100%;
  background: linear-gradient(to right, black 50%, white 50%);
  background-size: 200% 100%;
  background-position: right bottom;
  transition: all 0.5s ease-out;
}
.btn:hover {
  border: 1px solid black;
  background-position: left bottom;
  cursor: pointer;
}
#get-city {
  padding: 10px;
  font-size: 20px;
  border: 1px solid white;
  outline: none;
  width: 80%;
}
#get-city:focus {
  border: 1px solid black;
}
#locate-btn {
  width: 10%;
}
#search-btn {
  width: 10%;
}
#locate-btn:hover {
  color: cyan;
}
#search-btn:hover {
  color: blue;
}
</style>
