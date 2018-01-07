<template>
  <div>
    <button @click="substractMonth"> - </button>
    <!--Заменить на самописные date input позволяющие вводить только цифры-->
    <date-input
      :value="dateFrom"
      @input="changeDateFrom"
    />
    <date-input
      :value="dateTo"
      @input="changeDateTo"
    />
    <button @click="addMonth"> + </button>
    <button @click="close"> x </button>
    <br>
    <!--Отображение календаря повторяемое-->
    <table>
      <tr>
        <td>
          <slot name="buttons">
            <default-buttons @change="changeDateRange"/>
          </slot>
        </td>
        <template v-for="showedMonth in showedMonths">
          <td :key="showedMonth">
            <view-calendar
              :key="showedMonth"
              :showed-month="showedMonth"
              :date-to="dateTo"
              :tmp-date-to="tmpDateTo"
              :date-from="dateFrom"
              @click="chooseDay"
              @mouseoverDay="toggleTmpDay"
            />
          </td>
        </template>
      </tr>
    </table>
  </div>
</template>

<script>
  import CalendarDay from './CalendarDay'
  import ViewCalendar from './ViewCalendar'
  import DefaultButtons from './DateRangeButtons'
  import DateInput from './DateInput'

  import moment from 'moment'

  export default {
    name: 'DateRangePicker',
    components: {CalendarDay, ViewCalendar, DefaultButtons, DateInput},
    props: {
      // Показываемый месяц в виде строки распозноваемой moment.js
      showedMonth: {
        type: String,
        default: moment().format(this.dateFormat || 'DD.MM.YYYY')
      },
      dateFormat: {
        type: String,
        default: 'DD.MM.YYYY'
      },
      qtyShowCalendar: {
        type: Number,
        default: 1
      },
      dateFrom: {
        type: String,
        required: true
      },
      dateTo: {
        type: String,
        required: true
      }
    },

    data () {
      return {
        // Переход на другие месяцы, относительно showedMonth (чтобы работали кнопки сегодня и т.д.)
        monthShiftBy: 0,
        // Для отображения выбираемого периода по срабатыванию mouseOver
        tmpDateTo: ''
      }
    },

    computed: {
      showedMonths () {
        const {showedMonth, dateFormat, monthShiftBy} = this
        const momentDate = moment(showedMonth, dateFormat).startOf('month').add(monthShiftBy, 'month')
        // Написать нормальную генерацию array для повторения календарей
        return [0, 1].map(shifted => momentDate.add(shifted, 'month').format(dateFormat))
      },
      newChoosedCicle () {
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
        if (this.newChoosedCicle) {
          this.changeDateRange({
            dateTo: '',
            dateFrom: choosedDateStr
          })
          // this.dateFrom = choosedDateStr
          // this.dateTo = ''
        } else {
          // Добавить проверку какая дата больше, если меньше добавляемая сейчас, то поменять их местами
          const {dateFrom, dateFormat} = this
          const momentFrom = moment(dateFrom, dateFormat)
          const momentTo = moment(choosedDateStr, dateFormat)
          if (momentTo.isBefore(momentFrom)) {
            // Если даты перепутаны местами (вторая дата меньше первой)
            this.changeDateRange({
              dateTo: momentFrom.format(dateFormat),
              dateFrom: momentTo.format(dateFormat)
            })
            // this.dateFrom = momentTo.format(dateFormat)
            // this.dateTo = momentFrom.format(dateFormat)
          } else {
            // Если с датами все ок (вторая дата больше или равна первой)
            this.changeDateRange({
              dateTo: choosedDateStr,
              dateFrom: this.dateFrom
            })
            // this.dateTo = choosedDateStr
          }
        }
      },
      toggleTmpDay (DateStr) {
        if (!this.newChoosedCicle) {
          this.tmpDateTo = DateStr
        } else {
          this.tmpDateTo = ''
        }
      },
      changeDateFrom (newDateFrom) {
        this.changeDateRange({dateFrom: newDateFrom})
      },
      changeDateTo (newDateTo) {
        this.changeDateRange({dateTo: newDateTo})
      },
      changeDateRange ({dateTo, dateFrom}) {
        this.$emit('input', {dateTo, dateFrom})
      },
      close () {
        this.$emit('close')
      }
    }
  }
</script>
