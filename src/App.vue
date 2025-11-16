<template>
  <div id="app" :class="weatherClass">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div v-if="weather.main" class="weather-wrap">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
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

<script setup>
import { ref } from 'vue'

const api = {
  key: import.meta.env.VITE_API_KEY || 'ff0b88d56bd5cdde4db77a3fde1d199d',
  base: 'https://api.openweathermap.org/data/2.5/'
}

const query = ref('')
const weather = ref({})

function fetchWeather(e) {
  if (e.key === 'Enter') {
    fetch(`${api.base}weather?q=${query.value}&units=metric&APPID=${api.key}`)
      .then(res => res.json())
      .then(result => {
        weather.value = result
        query.value = ''
        console.log(result)
      })
  }
}

function dateBuilder(d) {
  const months = [
    'January', 'February', 'March', 'April', 'May', 'June',
    'July', 'August', 'September', 'October', 'November', 'December'
  ]
  const days = [
    'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'
  ]

  const day = days[d.getDay()]
  const date = d.getDate()
  const month = months[d.getMonth()]
  const year = d.getFullYear()

  return `${day} ${date} ${month} ${year}`
}

</script>

<style>
body
  .app-container {
    transition: background-color 0.5s ease;
    .bg-hot {background-color: #ff8c00;}
    .bg-very-hot { background-color: #ff4500;}
    .bg-cold {background-color: #4682b4;}
    .bg-default {background-color: #4a4a4a}
}



main {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.search-box {
  margin-bottom: 2rem;
}

.search-bar {
  padding: 10px;
  border-radius: 8px;
  border: none;
  width: 250px;
  outline: none;
}

.weather-box .temp {
  font-size: 48px;
  font-weight: bold;
}

.weather-box .weather {
  font-size: 24px;
  margin-top: 10px;
}

</style>