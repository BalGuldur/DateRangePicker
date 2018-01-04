<template>
  <div>
    <button @click="substractMonth"> - </button>
    <button @click="addMonth"> + </button>
    <!--Заменить на самописные date input позволяющие вводить только цифры-->
    <input :value="dateFrom">
    <input :value="dateTo">
    <br>
    <!--Отображение календаря повторяемое-->
    <view-calendar
      v-for="showedMonth in showedMonths"
      :key="showedMonth"
      :showed-month="showedMonth"
      :date-to="dateTo"
      :date-from="dateFrom"
      @click="chooseDay"
    />
  </div>
</template>

<script>
  import CalendarDay from './CalendarDay'
  import ViewCalendar from './ViewCalendar'

  import moment from 'moment'

  export default {
    name: 'DateRangePicker',
    components: {CalendarDay, ViewCalendar},
    props: {
      // Показываемый месяц в виде строки распозноваемой moment.js
      showedMonth: {
        type: String,
        default: moment().format(this.dateFormat)
      },
      dateFormat: {
        type: String,
        default: 'DD.MM.YYYY'
      },
      qtyShowCalendar: {
        type: Number,
        default: 1
      }
    },

    data () {
      return {
        // Переход на другие месяцы, относительно showedMonth (чтобы работали кнопки сегодня и т.д.)
        monthShiftBy: 0,
        dateFrom: '',
        dateTo: '',
        // Для отображения выбираемого периода по срабатыванию mouseOver
        tmpDateTo: ''
      }
    },

    computed: {
      showedMonths () {
        const {showedMonth, dateFormat, monthShiftBy} = this
        const momentDate = moment(showedMonth, dateFormat).startOf('month').add(monthShiftBy, 'month')
        return [0, 1].map(shifted => momentDate.add(shifted, 'month').format(dateFormat))
      },
      newDateChoosedCicle () {
        return ((this.dateFrom === '' && this.dateTo === '') || (this.dateFrom !== '' && this.dateTo !== ''))
      }
    },

    methods: {
      addMonth () {
        this.monthShiftBy = this.monthShiftBy + 1
      },
      substractMonth () {
        this.monthShiftBy = this.monthShiftBy - 1
      },
      chooseDay (choosedDateStr) {
        console.log('chooseDateStr', choosedDateStr)
        if (this.newDateChoosedCicle) {
          this.dateFrom = choosedDateStr
          this.dateTo = ''
        } else {
          // Добавить проверку какая дата больше, если меньше добавляемая сейчас, то поменять их местами
          this.dateTo = choosedDateStr
        }
      }
    }
  }
</script>

<style scoped>

</style>
