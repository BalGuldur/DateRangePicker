<template>
  <div id="app">
    <img src="./assets/logo.png">
    <date-range-input
      :date-from="dateFrom"
      :date-to="dateTo"
      @input="changeDateRange"
    />
  </div>
</template>

<script>
  // import HelloWorld from './components/HelloWorld'
  // import DateRangePicker from '@/components/DateRangePicker'
  import DateRangeInput from '@/components/DateRangeInput'
  // import DateMaskInput from '@/components/DateMaskInput'

  export default {
    name: 'App',
    components: { DateRangeInput },
    data () {
      return {
        dateFrom: '',
        dateTo: ''
      }
    },
    methods: {
      changeDateRange ({dateFrom, dateTo}) {
        this.dateFrom = dateFrom
        this.dateTo = dateTo
      }
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
