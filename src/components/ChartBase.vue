<template>
  <div class="chart-resize">
    <component
    v-bind:is="this.currentChartComponent"
    v-bind:dataCurrencies="this.dataCurrencies"
    v-bind:selectedCurrency="this.selectedCurrency"></component>
  </div>
</template>

<script>
import ChartLine from './ChartLine.vue'
import ChartBar from './ChartBar.vue'

export default {
  components: {
    'chart-line': ChartLine,
    'chart-bar': ChartBar
  },

  props: {
    dataCurrencies: {
      type: Object,
      required: true
    },

    selectedCurrency: {
      type: String,
      required: true
    },

    currentChartComponent: {
      type: String,
      required: true
    }
  }
}
</script>

<style lang="sass" scoped>
.chart-resize
  width: 75%
</style>