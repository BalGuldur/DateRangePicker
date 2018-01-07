<template>
  <table>
    <tr><td><button @click="today">Сегодня</button></td></tr>
    <tr><td><button @click="thisWeek">Эта неделя</button></td></tr>
    <tr><td><button @click="nextWeek">Следующая неделя</button></td></tr>
    <tr><td><button @click="thisMonth">Этот месяц</button></td></tr>
  </table>
</template>

<script>
  import moment from 'moment'

  export default {
    name: 'DateRangeButtons',
    props: {
      dateFormat: {
        type: String,
        default: 'DD.MM.YYYY'
      }
    },
    methods: {
      changeDate (dateRange) {
        this.$emit('change', dateRange)
      },
      today () {
        const date = moment().format(this.dateFormat)
        this.changeDate({dateFrom: date, dateTo: date})
      },
      thisWeek () {
        const dateFrom = moment().startOf('isoWeek').format(this.dateFormat)
        const dateTo = moment().endOf('isoWeek').format(this.dateFormat)
        this.changeDate({dateFrom, dateTo})
      },
      nextWeek () {
        const dateFrom = moment().add(1, 'week').startOf('isoWeek').format(this.dateFormat)
        const dateTo = moment().add(1, 'week').endOf('isoWeek').format(this.dateFormat)
        this.changeDate({dateFrom, dateTo})
      },
      thisMonth () {
        const dateFrom = moment().startOf('month').format(this.dateFormat)
        const dateTo = moment().endOf('month').format(this.dateFormat)
        this.changeDate({dateFrom, dateTo})
      }
    }
  }
</script>
