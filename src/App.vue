<template>
  <input
    type="text"
    placeholder="Search"
    v-model="query"
    @keypress="loadData"
  />
  <!-- <div v-if="weather?.main">{{ weather.name }}</div> -->
  <div>{{ weather.name }}</div>
</template>

<script>
import axios from "axios";
import { ref } from "vue";

export default {
  data() {
    return {
      apiKey: "064ddc5160fb4628cbb78268eb80bcf0",
      query: "",
      weather: {},
    };
  },
  methods: {
    async loadData(e) {
      if (e.key == "Enter") {
        const response = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&appid=${this.apiKey}`
        );
        this.weather = response.data;
        console.log(response.data);
      }
    },
  },
};
</script>

<style></style>
