<template>
  <div class="w-full mb-10 pt-10">
    <div class="mx-auto w-1/3 flex flex-col items-center text-center mb-10">
      <h2 class="text-3xl">Hello World!</h2>
      <h3 class="whitespace-nowrap text-2xl mb-6">Get your weather information here</h3>
      <h5 class="whitespace-nowrap">Powered by <a href="https://openweathermap.org/" target="_blank" class="text-blue-600">OpenWeather Weather API</a></h5>

      <div class="min-w-max px-16 py-2 rounded-md shadow bg-gray-200 mb-10">
        <h2 class="text-3xl">Enter Location</h2>
        <form v-on:submit.prevent="getElWeather" class="w-full">
            <div class="flex items-center py-1 border-b border-blue-600">
                <input class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none" v-model="location" type="text" placeholder="Ellensburg, WA">
                <button class="bg-blue-600 px-4 py-1 text-gray-200">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                    </svg>
                </button>
            </div>
        </form>
      </div>

      <TodayWeather v-if="showDash" :city="city" :currTemp="currTemp" :low="low" :high="high" :date="date" :description="description" />
    </div>
    <div>
      <h3 v-if="showDash" class="text-xl text-center mb-6">And here's how the rest of your week looks.</h3>
      <div class="flex flex-wrap justify-center mx-3 gap-3">
        <OtherWeather v-for="day in weekly" :key="day.id" :date="day.date" :low="day.low" :high="day.high" :description="day.description" />
      </div>
    </div>
  </div>
</template>

<script>
import TodayWeather from './components/TodayWeather.vue';
import OtherWeather from './components/OtherWeather.vue';
export default {
  components: {
    TodayWeather,
    OtherWeather
  },
  data() {
    return {
      showDash: false,
      location: '',
      city: '',
      CurrTemp: '',
      low: '',
      high: '',
      date: '',
      description: '',
      weekly: []
    }
  },
  methods: {
    getElWeather() {
      let vm = this;
      const myKey = import.meta.env.VITE_API_KEY;
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.location}&units=imperial&appid=${myKey}`)  
      .then(res => res.json())
      .then(data => {
        vm.city = data.name;
        vm.currTemp = data.main.temp;
        vm.low = data.main.temp_min;
        vm.high = data.main.temp_max;
        vm.description = data.weather[0].description;
        console.log(data);
        let lon = data.coord.lon;
        let lat = data.coord.lat;
        vm.showDash = true;
        let time = new Date(data.dt * 1000);
        vm.date = time.toDateString();
        
        fetch(`https://api.openweathermap.org/data/2.5/onecall?lat=${lat}&lon=${lon}&units=imperial&appid=${myKey}`)
        .then(res =>res.json())
        .then(data => {
          // console.log(data);
          const week = data.daily;
          console.log(week);
          for (const el of week) {
            const obj = {};
            // console.log(el);
            let date = new Date(el.dt *1000);
            // console.log(date.toDateString());
            if (vm.date === date.toDateString()) {
              continue;
            }
            obj.date = date.toDateString();
            obj.low = el.temp.min;
            obj.high = el.temp.max;
            obj.description = el.weather[0].description;
            vm.weekly.push(obj);
          }
          console.log(vm.weekly);
        })
        .catch(err => console.log(err));
      })
      .catch(err => console.log(err));
    }
  },
}
</script>