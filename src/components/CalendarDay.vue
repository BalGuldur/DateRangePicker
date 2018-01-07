<template>
  <button
    class="date"
    :class="{choosed: choosed, between: between, temped: temped, 'not-in-month': notInMonth}"
    @click="chooseDay"
    @mousein="toggleDay"
    @mouseout="stopToggleDay"
  >
    {{ view }}
  </button>
</template>

<script>
  import moment from 'moment'

  export default {
    name: 'CalendarDay',
    props: {
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
      },
      tmpDateTo: {
        type: String,
        default: ''
      },
      showedMonth: {
        type: String,
        required: true
      }
    },
    computed: {
      view () {
        return moment(this.value, this.dateFormat).format('DD')
      },
      notInMonth () {
        const monthOfDay = moment(this.value, this.dateFormat).month()
        return monthOfDay !== moment(this.showedMonth, this.dateFormat).month()
      },
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
      },
      temped () {
        const {tmpDateTo, dateFrom, dateFormat, value} = this
        if (tmpDateTo !== '' && dateFrom !== '') {
          let momentTo = moment(tmpDateTo, dateFormat)
          let momentFrom = moment(dateFrom, dateFormat)
          const momentDate = moment(value, dateFormat)
          if (momentTo.isBefore(momentFrom)) {
            return momentDate.isBetween(momentTo, momentFrom)
          } else {
            return momentDate.isBetween(momentFrom, momentTo)
          }
        } else {
          return false
        }
      }
    },
    methods: {
      chooseDay () {
        this.$emit('click', this.value)
      },
      toggleDay () {
        this.$emit('mouseoverDay', this.value)
      },
      stopToggleDay () {
        this.$emit('mouseendDay', this.value)
      }
    }
  }
</script>

<style>
  .not-in-month {
    color: darkgray !important;
  }

  .choosed {
    color: red;
  }

  .between {
    color: green;
  }

  .temped {
    color: greenyellow;
  }

  .date:hover {
    color: blue;
  }
</style>
