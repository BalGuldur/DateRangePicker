<template>
  <div>
    <template v-for="week in monthIterator">
      <calendar-day
        v-for="dayDate in week"
        :key="dayDate"
        :value="dayDate"
        :date-to="dateTo"
        :date-from="dateFrom"
        @click="(e) => $emit('click', e)"/>
      <br>
    </template>
    <br>
  </div>
</template>

<script>
  import CalendarDay from './CalendarDay'

  import moment from 'moment'

  export default {
    name: 'ViewCalendar',
    components: {CalendarDay},
    props: {
      showedMonth: {
        type: String,
        required: true
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
      dateFormat: {
        type: String,
        default: 'DD.MM.YYYY'
      }
    },
    computed: {
      // Первый отображаемый месяц. Двумерный массив содержащий даты по дням недели и по недели
      // Для примера
      // [
      // ['29.08', '30.08', '31.08', '1.09', '2.09', '3.09', '4.09'],
      // ['5.09', '6.09', '7.09', '8.09', '9.09', '10.09', '11.09'], ..., ..., ..., ...]
      monthIterator () {
        const {dateFormat, showedMonth} = this
        const momentDate = moment(showedMonth, dateFormat)
        return [0, 1, 2, 3, 4, 5].map(weekNumber => {
          return [1, 2, 3, 4, 5, 6, 7].map(dayNumber => {
            return momentDate
              .isoWeek(weekNumber + this.firstWeek)
              .isoWeekday(dayNumber)
              .format(dateFormat)
          })
        })
      },
      startOfMonth () {
        const {showedMonth, dateFormat} = this
        const momentDate = moment(showedMonth, dateFormat)
        return momentDate.startOf('month')
      },
      firstWeek () {
        return this.startOfMonth.isoWeek()
      }
    }
  }
</script>

<style scoped>

</style>
