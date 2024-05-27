<template>
  <div
    :class="bgColor"
    class="w-full h-[100vh]  p-4 flex flex-col"
  >
    <input
      type="text"
      placeholder="Search for a city"
      v-model="query"
      @keypress="loadData"
      class="w-full h-10 rounded-md p-3 outline-none mt-5 text-slate-500 mx-auto lg:w-[800px]"
    />
    <section
      class="mt-14 flex flex-col items-center gap-5 text-center"
      v-if="!loading || typeoff"
    >
      <div class="flex flex-col">
        <h2>{{ weather?.name }},{{ weather?.sys?.country }}</h2>
        <h3></h3>
      </div>
      <h1>{{ Math.round(weather?.main?.temp) }}°c</h1>
      <h2>{{ weather.weather[0].main }}</h2>
    </section>
    <h2 class="mx-auto mt-20" v-else>Allow a location or search for a city</h2>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      apiKey: "064ddc5160fb4628cbb78268eb80bcf0",
      query: "",
      weather: {},
      location: null,
      loading:true,
    };
  },
  created(){
    this.getLocation()
  },
  methods: {
    
    async loadData(e) {
      if (e.key == "Enter" && this.query !='') {
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&appid=${this.apiKey}`
        );
        this.weather = response.data;
        this.loading = false;
      }
    },
    async loadCurrentData(){
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?lat=${this.location.latitude}&lon=${this.location.longitude}&appid=${this.apiKey}&units=metric`
        );
        this.weather = response.data;
        this.loading = false;
    },
    getLocation(){
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          position => {
            this.location = {
              latitude: position.coords.latitude,
              longitude: position.coords.longitude
            };
            this.loadCurrentData()
          },
          error => {
            console.error('Error getting location:', error);
            this.loading = false;
          }
        );
      }
    }
  },
  mounted(){
    console.log();
  },
  computed: {
    typeoff() {
      return typeof this.weather.main != "undefined";
    },
    bgColor() {
      if (this.weather?.main?.temp > 16) {
        return "bg-yellow-300";
      } else {
        return "bg-cyan-200";
      }
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  color: white;
  font-family: "monserrat", sans-serif;
}
</style>
