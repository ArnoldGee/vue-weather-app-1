<template>
  <main class="container-sm">
    <h1>Quin temps fa a... ?</h1>
    <div class="input-group mb-3">
      <input
        class="form-control"
        v-model="city"
        placeholder="Enter city name..."
      />
      <button class="btn btn-outline-primary" v-if="city" @click="handleSubmit">
        Cerca
      </button>
    </div>
    <div v-if="weatherData" class="">
      <div class="row">
        <div class="col-md">
          <h3>Temps</h3>
          <img
            className="city-icon"
            v-bind:src="imageSrc"
            v-bind:alt="weatherData.weather[0].description"
          />
          <p>{{ weatherData.weather[0].description }}</p>
        </div>
        <div class="col-md">
          <h3>Temperatura</h3>
          <ul>
            <li>Temperatura: {{ weatherData.main.temp }}</li>
            <li>Màxima: {{ weatherData.main.temp_max }}</li>
            <li>Mínima: {{ weatherData.main.temp_min }}</li>
          </ul>
        </div>
      </div>
    </div>
  </main>
  <footer class="container-fluid">Fet amb Vue.js per Arnau Gómez</footer>
</template>

<script>
export default {
  data() {
    return {
      city: "Barcelona",
      weatherData: null,
    };
  },
  methods: {
    handleSubmit() {
      console.log(this.city);
      this.fetchWeatherData(this.city);
      this.city = "";
    },
    async fetchWeatherData(city) {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=2403f365fec7fa68ec90bbd1eff360ed&lang=ca&units=metric`
      );
      this.weatherData = await response.json();
    },
  },
  computed: {
    imageSrc() {
      return this.weatherData
        ? `https://openweathermap.org/img/wn/${this.weatherData.weather[0].icon}@2x.png`
        : "";
    },
  },
};
</script>

<style>
#app {
  margin-top: 4rem;
}
h1, h3 {
  margin-top: 2rem;
  margin-bottom: 1rem;
}
footer {
  position: absolute;
  bottom: 0;
  padding: 3rem;
  text-align: center;
  }
.container-sm {
  max-width: 700px;
  }
</style>
