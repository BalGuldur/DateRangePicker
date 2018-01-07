<template>
  <input
    :value="value"
    @change="changeDate"
  >
</template>

<script>
  import moment from 'moment'

  export default {
    name: 'DateInput',
    props: {
      value: {
        type: String,
        required: true
      },
      dateFormat: {
        type: String,
        default: 'DD.MM.YYYY'
      }
    },
    methods: {
      changeDate (e) {
        const newValue = e.target.value
        const newDate = moment(newValue, this.dateFormat)
        if (newDate.isValid()) {
          this.$emit('input', newDate.format(this.dateFormat))
        } else {
          this.$el.value = this.value
        }
      }
    }
  }
</script>

<style scoped>

</style>
