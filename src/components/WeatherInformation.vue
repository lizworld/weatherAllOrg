<template>
  <div class="container">
    <div class="inner">
      <h2>
        {{temp}} <span>&#176;</span>
      </h2>
      <p>
        {{description}}
      </p>

      <div v-if="cloudy" class="weather-condition">
        <img src="../assets/cloudy_sun.png" alt="">
      </div>
      <div v-if="rainy" class="weather-condition">
        <img src="../assets/rain.png" alt="">
      </div>
      <div v-if="sunny" class="weather-condition">
        <img src="../assets/sunny.png" alt="">
      </div>
      <div v-if="noIcon" class="condition">
        <p>no Icon </p>
      </div>
      <div class="input_container">
        <div>

          <label for="lat">위도</label>
          <input type="text" id="name" v-model="lat">
        </div>
        <div>

          <label for="lon">경도</label>
          <input type="text" id="name" v-model="lon">
        </div>
        <button @click="GetWeatherData">getData</button>

      </div>
    </div>
  </div>
</template>
<script>
  import axios from 'axios'
  export default {
    data() {
      return {
        weather: {},
        description: '',
        cloudy: false,
        rainy: false,
        stormy: false,
        sunny: false,
        temp: '',
        lat: '38',
        lon: '78',
        noIcon: false
      }
    },
    mounted() {

      this.GetWeatherData()
    },
    methods: {
      GetWeatherData() {
        axios.get('http://api.openweathermap.org/data/2.5/weather?lat=' + this.lat + '&lon=-' + this.lon + '&appid=209427778b168dd3cfb8c77b931a8344&units=metric')
          .then(response => {
            console.log(response.data);

            let crtTemp = response.data.main.temp
            let mainDescription = response.data.weather[0].description
            this.description = mainDescription

            switch (true) {
              case 'Clear':
                this.cloudy = false
                this.sunny = true
                this.stormy = false
                this.rainy = false
                this.noIcon = false
                break;
              case 'Thunderstorm':
                this.cloudy = false
                this.sunny = false
                this.noIcon = false
                this.stormy = true
                this.rainy = false
                break;
              case 'Clouds':
                this.cloudy = true
                this.sunny = false
                this.stormy = false
                this.rainy = false
                this.noIcon = false
                break;
              case 'Rain':
                this.cloudy = false
                this.sunny = false
                this.stormy = false
                this.rainy = true
                this.noIcon = false
                break;

              default:
                this.noIcon = true
            }


            this.temp = crtTemp
          })
          .catch(error => {
            console.log(error)
          })
      }
    },
  }
</script>
<style>
  .container {
    display: flex;
    justify-content: center;
  }

  .inner {
    width: 80%;
    /* background-color: rgb(192, 192, 255); */
  }

  p {
    text-transform: capitalize;
  }
</style>