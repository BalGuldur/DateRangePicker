<template>
  <div>
    <input
      :value="value"
      @change="changeValue"
      @focus="pickerIsOpen = true"
    >
    <template v-if="pickerIsOpen">
      <date-range-picker
        :date-to="dateTo"
        :date-from="dateFrom"
        @input="changeDateRange"
        @close="pickerIsOpen = false"
      />
    </template>
  </div>
</template>

<script>
  import DateRangePicker from './DateRangePicker'

  import moment from 'moment'

  export default {
    name: 'DateRangeInput',
    components: { DateRangePicker },
    props: {
      dateFrom: {
        type: String,
        required: true
      },
      dateTo: {
        type: String,
        required: true
      },
      dateFormat: {
        type: String,
        default: 'DD.MM.YYYY'
      }
    },
    data () {
      return {
        value: '',
        pickerIsOpen: false
      }
    },
    watch: {
      dateTo () {
        const {dateTo, dateFrom} = this
        this.value = dateFrom + ' - ' + dateTo
      },
      dateFrom () {
        const {dateTo, dateFrom} = this
        this.value = dateFrom + ' - ' + dateTo
      }
    },
    methods: {
      changeValue (e) {
        const value = e.target.value
        const split = value.split('-')
        const from = moment((split[0] || '').trim(), this.dateFormat)
        const to = moment((split[1] || '').trim(), this.dateFormat)
        if (from.isValid() && to.isValid()) {
          this.changeDateRange({dateTo: to.format(this.dateFormat), dateFrom: from.format(this.dateFormat)})
        } else {
          const {dateFrom, dateTo} = this
          this.value = ''
          this.value = dateFrom + ' - ' + dateTo
          this.changeDateRange({dateTo, dateFrom})
        }
      },
      changeDateRange ({dateTo, dateFrom}) {
        this.$emit('input', {dateTo, dateFrom})
      }
    }
  }
</script>
