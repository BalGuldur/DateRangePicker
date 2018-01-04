<template>
  <button
    :class="{choosed: choosed, between: between}"
    @click="chooseDay"
    @mouseover="toggleDay"
  >
    {{ value }}
  </button>
</template>

<script>
  import moment from 'moment'

  export default {
    name: 'CalendarDay',
    props: {
      // dayNumber: {
      //   type: Number,
      //   required: false
      // },
      value: {
        type: String,
        required: true
      },
      dateFormat: {
        type: String,
        default: 'DD.MM.YYYY'
      },
      dateFrom: {
        type: String,
        default: ''
      },
      dateTo: {
        type: String,
        default: ''
      }
    },
    computed: {
      choosed () {
        return (this.value === this.dateTo || this.value === this.dateFrom)
      },
      between () {
        const {dateTo, dateFrom, value, dateFormat} = this
        if (dateTo !== '' && dateFrom !== '') {
          const momentTo = moment(dateTo, dateFormat)
          const momentFrom = moment(dateFrom, dateFormat)
          return moment(value, dateFormat).isBetween(momentFrom, momentTo)
        } else {
          return false
        }
      }
    },
    methods: {
      chooseDay () {
        console.log('chooseDay', this.value)
        this.$emit('click', this.value)
      },
      toggleDay () {
        console.log('toggleDay', this.value)
        this.$emit('mouseover', this.value)
      }
    }
  }
</script>

<style scoped>
  .choosed {
    color: red;
  }

  .between {
    color: green;
  }
</style>
