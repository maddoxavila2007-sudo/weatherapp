<template>
  <div id="app">
    
    <main> 
      <div class="searchbox">
        <input type="text" 
        class="search-bar" 
        placeholder="Search..." 
        v-model="query" 
        @keypress="fetchWeather"/>
      </div>

      <div v-if="weather.main" class="weather-wrap">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder(new Date()) }}</div>
        </div>
        
        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp) }}°C
          </div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>

  </div>
</template>

<script>
  import {ref} from 'vue';
  const api = {key: import.meta.env.VITE_API_KEY, 
  base: "https://api.openweathermap.org/data/2.5/"}
  const quary = ref('');
  const weather = ref({});

  function fetchWeather(e) {
    if (e.key === 'Enter') {
      fetch(`${api.base}weather?q=${query.value}&units=metric&APPID=${api.key}`)
    .then(res => res.json())
    .then(result => {
      weather.value = result;
      query.value = '';
      console.log(result);
    });
    }
  }

function dateBuilder(d) {
  let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
  let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

  let day = days[d.getDay()];
  let date = d.getDate();
  let month = months[d.getMonth()];
  let year = d.getFullYear();

   return `${days[d.getDay()]} ${d.getDate()} ${months[d.getMonth()]} ${d.getFullYear()}`
}

</script>