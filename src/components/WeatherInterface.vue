<script setup>
import {computed, ref, watch} from 'vue';

const props = defineProps({
  city: String,
  state: String,
})

const isLoading = ref();
const result = ref();
const weatherMain = ref();
const apiKey = "00bca121ffc3e7002d957782035ab63f";
const endpoint = {weather: `https://api.openweathermap.org/data/2.5/weather?q=${props.city},${props.state}&appid=${apiKey}`}

const getWeather = async () => {
  try {
    isLoading.value = true;

    const response = await fetch(endpoint.weather);
    result.value = await response.json();
    weatherMain.value = (result.value.weather[0].main).toLowerCase();
    isLoading.value = false;
    console.log(result.value, weatherMain.value);
  } catch (error) {
    result.value = 404
    isLoading.value = false;
    console.log(error);
  }
}


watch(props, (newProps) => {
  if (newProps.city === undefined) return;
  endpoint.weather = `https://api.openweathermap.org/data/2.5/weather?q=${newProps.city},${newProps.state},BR&units=metric&appid=${apiKey}`;
  getWeather();
}, {immediate: true});

const weatherOpt = {
  clouds: './images/cloud-computing.png',
  clear: './images/sun.png',
  rain: './images/heavy-rain.png',
}


const getImageByWeatherMain = computed(() => {
  switch (weatherMain.value) {
    case 'clouds':
      return weatherOpt.clouds;
    case 'clear':
      return weatherOpt.clear;
    case 'rain':
      return weatherOpt.rain;
    default:
      console.log(weatherMain.value)
      return;
  }
})


</script>

<template>
  <div class="card">
    <h3 v-if="isLoading">Carregando clima em {{ props.city }}</h3>

    <div v-else>
      <h3 v-if="result === 404">Não foi possível encontrar {{ props.city }}</h3>
      <div v-else>
        <h3>Clima em {{ props.city }}</h3>
        <div class="flex">
          <img :src="getImageByWeatherMain" width="50" height="50" style="margin-right: 15px">
          <h1 style="margin: 0">{{ (Math.round(result.main.temp)) }} °C</h1>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
.flex{
  display: flex;
  align-content: center;
  justify-content: center;
}
</style>