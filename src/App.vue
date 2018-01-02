<template>
  <div id="app">
    <img src="./assets/logo.png">
    <date-range-picker showed-month="03.01.2018"/>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld'
import DateRangePicker from '@/components/DateRangePicker'

export default {
  name: 'App',
  components: {
    DateRangePicker
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
