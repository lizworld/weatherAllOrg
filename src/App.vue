<template>
  <div id="app">
    <Weather/>
  </div>
</template>

<script>
import Weather from './components/Weather'

export default {
  name: 'App',
  components: {
    Weather
  },

}
</script>

<style>
@import url(//spoqa.github.io/spoqa-han-sans/css/SpoqaHanSansNeo.css);
*{margin:0;padding:0;box-sizing: border-box;font-family: 'Spoqa Han Sans Neo', 'sans-serif';}
#app {

}
</style>
