<template>
  <div class="p-6 max-w-md mx-auto bg-white rounded-lg shadow-lg">
    <h1 class="text-2xl font-bold text-center mb-4">Hava Durumu</h1>
    <input
      type="text"
      v-model="city"
      placeholder="Şehir adı"
      class="w-full border p-2 rounded-lg mb-4"
    />
    <button
      @click="fetchWeather"
      class="w-full bg-blue-500 text-white p-2 rounded-lg"
    >
      Hava Durumunu Getir
    </button>
    <div v-if="weather" class="mt-6">
      <lottie-player
        :src="animationSrc"
        background="transparent"
        speed="1"
        style="width: 150px; height: 150px;"
        loop
        autoplay
      ></lottie-player>
      <p class="text-center mt-4">Şehir: {{ weather.name }}</p>
      <p class="text-center">Sıcaklık: {{ weather.main.temp }}°C</p>
      <p class="text-center">Durum: {{ weather.weather[0].description }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      weather: null,
      animationSrc: '',
    };
  },
  methods: {
    async fetchWeather() {
      const API_KEY = 'API_KEYİNİZ';
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${API_KEY}`;
      try {
        const response = await axios.get(url);
        this.weather = response.data;

        const weatherMain = this.weather.weather[0].main.toLowerCase();
        if (weatherMain.includes('rain')) {
          this.animationSrc = '/animations/rainy.json';
        } else {
          this.animationSrc = '/animations/sunny.json';
        }
      } catch (error) {
        alert('Hava durumu alınamadı.');
      }
    },
  },
};
</script>
