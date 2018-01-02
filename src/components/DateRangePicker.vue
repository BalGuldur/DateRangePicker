<template>
  <div>
    <button @click="substractMonth"> - </button>
    <button @click="addMonth"> + </button>
  </div>
</template>

<script>
  import moment from 'moment'

  export default {
    name: 'DateRangePicker',
    props: {
      // Показываемый месяц в виде строки распозноваемой moment.js
      showedMonth: {
        type: String,
        default: moment().format(this.dateFormat)
      },
      dateFormat: {
        type: String,
        default: 'DD.MM.YYYY'
      }
    },

    data () {
      return {
        // Переход на другие месяцы, относительно showedMonth (чтобы работали кнопки сегодня и т.д.)
        monthShiftBy: 0
      }
    },

    computed: {
      // Первый отображаемый месяц. Двумерный массив содержащий даты по дням недели и по недели
      // Для примера
      // [
      // ['29.08', '30.08', '31.08', '1.09', '2.09', '3.09', '4.09'],
      // ['5.09', '6.09', '7.09', '8.09', '9.09', '10.09', '11.09'], ..., ..., ..., ...]
      firstMonth () {
        const {monthShiftBy} = this
        let momentDate = moment(this.showedMonth, this.dateFormat).add(monthShiftBy, 'month')
        return [0, 1, 2, 3, 4, 5].map(weekNubmer => {
          return [1, 2, 3, 4, 5, 6, 7].map(dayNumber => {
            return momentDate
              .isoWeek(weekNubmer + this.firstWeekFirstMonth)
              .isoWeekday(dayNumber)
              .format(this.dateFormat)
          })
        })
      },
      // Воторой отображаемый месяц, формат аналогичен первому отображаемому месяцу
      secondMonth () {
        const {monthShiftBy} = this
        let momentDate = moment(this.showedMonth, this.dateFormat).add(monthShiftBy, 'month')
        return [0, 1, 2, 3, 4, 5].map(weekNumber => {
          return [1, 2, 3, 4, 5, 6, 7].map(dayNumber => {
            return momentDate
              .isoWeek(weekNumber + this.firstWeekSecondMonth)
              .isoWeekday(dayNumber)
              .format(this.dateFormat)
          })
        })
      },
      startOfFirstMonth () {
        const {monthShiftBy} = this
        let momentDate = moment(this.showedMonth, this.dateFormat).add(monthShiftBy, 'month')
        return momentDate.startOf('month')
      },
      startOfSecondMonth () {
        const {monthShiftBy} = this
        let momentDate = moment(this.showedMonth, this.dateFormat).add(monthShiftBy, 'month')
        return momentDate.add(1, 'M').startOf('month')
      },
      // Номер первой недели месяца в году (для первого месяца)
      firstWeekFirstMonth () {
        return this.startOfFirstMonth.isoWeek()
      },
      // Номер первой недели месяца в году (для второго месяца)
      firstWeekSecondMonth () {
        return this.startOfSecondMonth.isoWeek()
        // return this.startOfSecondMonth.isoWeek()
      }
    },

    methods: {
      addMonth () {
        this.monthShiftBy = this.monthShiftBy + 1
      },
      substractMonth () {
        this.monthShiftBy = this.monthShiftBy - 1
      }
    }
  }
</script>

<style scoped>

</style>
