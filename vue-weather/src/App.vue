<template>
  <div class="w-full">
    <div class="mx-auto w-1/3 flex flex-col items-center text-center">
      <h3 class="text-xl">Hello World!</h3>
      <h1 class="whitespace-nowrap">Get your weather information here</h1>

      <div class="w-max px-16 py-2 rounded-md shadow bg-gray-200 mb-10">
        <h3 class="text-xl">Enter Location</h3>
        <form v-on:submit.prevent="getElWeather" class="w-full">
            <div class="flex items-center py-1 border-b border-blue-600">
                <input class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none" v-model="location" type="text" placeholder="Ellensburg, WA">
                <button class="bg-blue-600 px-6 py-1 text-gray-200">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                    </svg>
                </button>
            </div>
        </form>
      </div>

      <TodayWeather v-if="showDash" :city="city" :currTemp="currTemp" :low="low" :high="high" />
    </div>
  </div>
</template>

<script>
import TodayWeather from './components/TodayWeather.vue';
export default {
  components: {
    TodayWeather
  },
  data() {
    return {
      showDash: false,
      location: '',
      city: '',
      CurrTemp: '',
      low: '',
      high: '',
      weekly: {}
    }
  },
  methods: {
    getElWeather() {
      let vm = this;
      const myKey = 'niceTry';
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.location}&units=imperial&appid=${myKey}`)  
      .then(res => res.json())
      .then(data => {
        vm.city = data.name;
        vm.currTemp = data.main.temp;
        vm.low = data.main.temp_min;
        vm.high = data.main.temp_max;
        console.log(data);
        let lon = data.coord.lon;
        let lat = data.coord.lat;
        vm.showDash = true;
        let time = new Date(data.dt * 1000);
        console.log(time.toDateString());
        
        fetch(`https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${lon}&units=imperial&appid=${myKey}`)
        .then(res =>res.json())
        .then(data => {
          console.log(data);
          const week = data.daily;
          console.log(week);
          for (const el of week) {
            console.log(el);
          }
        })
        .catch(err => console.log(err));
      })
      .catch(err => console.log(err));
    }
  },
}
</script>