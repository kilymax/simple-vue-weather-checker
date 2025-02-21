<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return this.city
    },
    showTemp() {
      return `Температура: ${this.info?.temp}`
    },
    showPressure() {
      return `Давление: ${Math.round(this.info?.pressure * 0.75008)}`
    }

  },
  methods: {
    getWeather() {
      if(this.city.trim().length < 2) {
        this.error = "Название должно быть длиной более 1 символа"
        return false
      }
      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=1d05b91941dcce2f43c6c4e0dd3603c1`)
        .then(res => (this.info = res?.data?.main))
    }
  }
}
</script>

<template>
  <div class="weather-window">
    <div class="weather-window__wrapper">
      <h1>Weather App on Vue</h1>
      <p>Be in touch with climate in {{ city === "" ? 'your place' : cityName }}</p>
      <input type="text" v-model="city" placeholder="Город">
      <button v-if="city !== ''" @click="getWeather()" class="weather-window__button">Узнать погоду</button>
      <button disabled v-else class="weather-window__button">Необходимо указать город</button>
      <p class="error">{{ error }}</p>
      <div v-show="info != null" class="results">
        <span>{{ showTemp }}</span>
        <span>{{ showPressure }}</span>
      </div>
    </div>
  </div>

</template>

<style scoped lang="scss">
.weather-window {
  width: 80%;
  max-width: 500px;
  margin-inline: auto;

  &__wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 500px;
    border-radius: 25px;
    padding: 20px;
    background-color: rgba(0, 0, 0, 0.268);
    box-shadow: 0px 0px 8px 1px;

    .error {
      margin-top: 15px;
      color: red;
    }

    input {
      margin-top: 30px;
      font-size: 14px;
      padding: 8px 12px;
      background: rgba($color: #000000, $alpha: 0.1);
      border: 2px solid white;
      border-radius: 6px;
      color: white;
      outline: none;

      &:focus {
        border-color: white;
      }
    }

    .results {
        display: flex;
        flex-direction: column;
        align-items: center;
      }
  }

  &__button {
      color: white;
      padding: 8px 14px;
      margin-top: 30px;
      background-color: transparent;
      border: 0;
      border: 1px solid white;
      border-radius: 15px;
      cursor: pointer;

      &:disabled {
        color: red;
        border-color: red;
        pointer-events: none;;
      }

      &:hover {
        background-color: rgba($color: #ffffff, $alpha: 1.0);
        color: black;
        transition-duration: 0.5s;
      }
    }
}


</style>
