<template>
  <div class="v-result">
    <div class="header">
      <h1 class="result-header__item">
        Vue Weather App
      </h1>
    </div>
    <div class="result__info">
      <div class="result-info__item">
        <div class="temp">{{ celsius }}°C</div>
        <div class="description">| {{ weatherMainDescription.toUpperCase() }}</div>
      </div>
      <div class="result-info__icon">

      </div>
    </div>
    <h2 class="result__city">{{ city }}</h2>
    <div class="result__detailed">
      <h6 class="result-detailed__header">Weather Info</h6>
      <div class="result-detailed__item">
        <div class="card">
          <div class="card__icon"></div>
          <div class="card__info">
            <div>{{ tomorrow }}</div>
            <div>Date</div>
          </div>
        </div>
        <div class="card">
          <div class="card__icon"></div>
          <div class="card__info">
            <div>{{ humidity }}</div>
            <div>Humidity</div>
          </div>
        </div>
        <div class="card">
          <div class="card__icon"></div>
          <div class="card__info">
            <div>{{ windSpeed }}</div>
            <div>Wind</div>
          </div>
        </div>
        <div class="card">
          <div class="card__icon"></div>
          <div class="card__info">
            <div>{{ pressure }}</div>
            <div>Pressure</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import axios from "axios";
import {format} from "date-fns";

export default {
  name: "v-result",
  data() {
    return {
      city: "",
      celsius: "",
      weatherMainDescription: "",
      tomorrow: "",
      windSpeed: "",
      humidity: "",
      pressure: "",
    }
  },
  methods: {
    async postFetch() {
      try {
        const response = await axios.get("https://api.openweathermap.org/data/2.5/weather?q=Buynaksk,ru&appid=d374e8f9cbae209a96ed51df9e9e5721&units=metric");
        this.city = response.data.name;
        this.celsius = Math.ceil(response.data.main.temp);
        this.weatherMainDescription = response.data.weather[0].description;
        this.windSpeed = response.data.wind.speed;
        this.humidity = response.data.main.humidity;
        this.pressure = response.data.main.pressure;
        console.log(response.data)
      } catch {
        alert("Error")
      }
    },
    newDateFunc() {
      const dateFormat = format(new Date(), "dd/MMMM");
      this.tomorrow = dateFormat.replaceAll("/", ".");
    }
  },
  mounted() {
    this.postFetch();
    this.newDateFunc();
  }
}
</script>

<style scoped>
.result-header__item {
  font-size: 18px;
  font-weight: 500;
  margin-bottom: 32px;
}

.result__info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 64px;
}

.result-info__item {
  display: flex;
}

.result-info__item {
  display: flex;
  align-items: center;
}

.temp {
  font-size: 28px;
  font-weight: 500;
  margin-right: 8px;
}

.result-detailed__header {
  font-size: 18px;
  font-weight: 500;
  text-align: left;
  margin-bottom: 32px;
  margin-top: 16px;
}

.result-detailed__item {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32px;
}

.description {
  font-size: 14px;
}

.result__city {
  font-weight: 500;
}
</style>