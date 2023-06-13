<template>
  <div id="container" >
    <h1>Todays Reports</h1>
    <img :src="icon" alt="" />
    <h2> <span>{{ city }}</span> 's weather <br />
      <b> {{ dsc }}</b>
    </h2>
    <p class="temp">temp: {{ temp }}<span>&#176;</span></p>
    <form v-on:submit.prevent="weatherData" >

      <h3>

        <span> 도시</span>를 입력하세요
      </h3>
      <input type="text" v-model="city"/>
      <!-- <button @click="weatherData">확인</button> -->
    </form>
  </div>
</template>

<script>
  import axios from "axios";
  import background from "@/assets/background";
  export default {
    data() {
      return {
        bg: background,
        icon: "",
        dsc: "",
        temp: "",
        city: "seoul",
        mainDsc: ''
      };
    },

    mounted() {

      this.weatherData();

    },

    methods: {
      weatherData() {

        axios
          .get(
            " https://api.openweathermap.org/data/2.5/weather?q=" + this.city + "&appid=209427778b168dd3cfb8c77b931a8344&units=metric"
          )
          .then((response) => {

            let data = response.data;

            console.log(this.city)
            this.dsc = data.weather[0].main;
            // main 설명글
            this.mainDsc = data.weather[0].main
            this.temp = data.main.temp;

            this.icon = "https://openweathermap.org/img/wn/" + data.weather[0].icon + "@2x.png";

            const container = document.querySelector('#container')

            for (let i = 0; i < this.bg.length; i++) {

              if (this.mainDsc == this.bg[i].main) {
                container.style.background = this.bg[i].background
              }
              // else {

              // }
            }

          })
          .catch((error) => {
            console.log(error);
          });
      },
    },
  };
</script>

<style>
  #container {
    text-align: center;
    color: #fff;
    background: rgb(37, 46, 148);
    background: radial-gradient(circle,
        rgba(37, 46, 148, 1) 0%,
        rgba(0, 0, 0, 1) 100%);
    min-height: 90vh;
    text-align: center;
    padding: 25vh 0;
    position: relative;
  }

  h1 {
    font-size: 40px;
    font-weight: 600;
  }

  form {
    bottom: 0;
    position: absolute;
    width: 100%;
    border-radius: 40px 40px 0 0;
    background-color: #fff;
    box-shadow: 0 -20px 20px rgba(0, 0, 0, 0.144);
    color: #000;
    padding: 50px 0;
  }

  h2 {
    font-weight: 200;
    font-size: 30px;
  }
  h3 {
    font-weight: 200;
    font-size: 20px;
  }

  span {
    font-weight: 400;
  }

  input {
    border-left: none;
    border-right: none;
    border-top: none;
    display: inline-block;
    line-height: 1.6;
    font-size: 20px;
    padding-top: 20px;
    text-align: center;
    outline: none;
  }

  img {
    width: 100px;
  }
</style>