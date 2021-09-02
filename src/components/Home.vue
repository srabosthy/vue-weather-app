<template>
  <div class="bg-gray-100 min-h-screen">
    <div class="text-center mt-1 bg-gray-100 p-4 shadow-lg rounded-xl mx-auto max-w-sm">
      <h2 class=" text-3xl font-bold text-gray-600"> Vue Weather</h2>
    </div>
    <form @submit.prevent="getWeather" :class="[ error ? 'bg-red-300 animate-pulse' : 'bg-gray-300' ]" class="text-center mt-2  p-4 shadow-lg rounded-xl mx-auto max-w-sm">
      <input type="text"  placeholder="Search City" v-model="citySearch" :class="[ error ? 'bg-red-200 animate-pulse' : 'bg-gray-200' ]" class="shadow-md text-gray-600 rounded-md p-1 w-4/5 text-center">
    </form>
    <!-- <div v-if="error" class="text-center animate-pulse mt-1 bg-red-300 opacity-20 p-4 shadow-2xl rounded-xl mx-auto max-w-sm">
      <h2 class=" text-md font-bold text-black"> Wrong City! </h2>
    </div> -->
    <div class="text-center mt-2 bg-gray-300 p-4 shadow-lg rounded-xl mx-auto max-w-sm">
      <h2 class=" text-gray-600 font-normal text-2xl">{{ city }}</h2>
      <h2 class=" text-gray-500 font-light text-sm ">{{ condition }}</h2>
      <h2 class="text-6xl font-light text-gray-600">{{ Math.round(temp) }}°</h2>
      <h2 class=" text-gray-500 text-sm">H:{{ Math.round(max_temp) }}° L:{{ Math.round(min_temp) }}°</h2>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      error: '',
      accesKey : "5299e83c9cf2fef510fb8ddaa3206ac8",
      city : 'dhaka',
      citySearch : '',
      condition : '',
      temp : '',
      max_temp:'',
      min_temp:'',
    }
  },
  async created() {
    const url = 'https://api.openweathermap.org/data/2.5/weather?q='+this.city+'&appid='+this.accesKey+'&units=metric';
    const result = await axios.get(url);
    const response = result.data;
    this.temp = response.main.temp;
    this.condition = response.weather[0].description;
    this.max_temp = response.main.temp_max;
    this.min_temp = response.main.temp_min;
  },
  // mounted() {
  //   axios.get(this.url).then((result) => {
  //     const response = result.data;
  //     this.temp = response.main.temp;
  //     this.condition = response.weather[0].description;
  //   })
  // }

  methods: {
    async getWeather() {
      // console.log(this.citySearch);
      try {
        const url = 'https://api.openweathermap.org/data/2.5/weather?q='+this.citySearch+'&appid='+this.accesKey+'&units=metric';
        const result = await axios.get(url);
        const response = result.data;
        this.temp = response.main.temp;
        this.condition = response.weather[0].description;
        this.max_temp = response.main.temp_max;
        this.min_temp = response.main.temp_min;
        this.city = this.citySearch;
        this.error = '';
        this.citySearch = '';
      } catch (error) {
        this.error = error.message;
        console.log(error.message);
      }
    },
  }
}
</script>