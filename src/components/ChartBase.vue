<template>
  <div class="chart-resize">
    <component
    v-bind:is="this.currentChartComponent"
    v-bind:dataCurrencies="this.dataCurrencies"
    v-bind:selectedCurrency="this.selectedCurrency"
    v-bind:weekDays="this.weekDays"></component>
  </div>
</template>

<script>
import ChartLine from './ChartLine.vue'
import ChartBar from './ChartBar.vue'
import ChartRadar from './ChartRadar.vue'
import ChartHorizontalBar from './ChartHorizontalBar.vue'
import ChartPie from './ChartPie.vue'

export default {
  components: {
    'chart-line': ChartLine,
    'chart-bar': ChartBar,
    'chart-radar': ChartRadar,
    'chart-horizontal-bar': ChartHorizontalBar,
    'chart-pie': ChartPie
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
  },

  data() {
    return {
      weekDays: []
    }
  },

  created() {
    this.getWeekDays()
  },

  methods: {
    getWeekDays() {
      let weekDays = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      let days = [];
  
      for (let i = 0; i < 7; i++) {
        // Получаем названий дней недели за последние 7 дней
        let date = new Date();

        days.push( date.getDay( date.setDate( date.getDate()-i ) ) );
        this.weekDays.unshift( weekDays[days[i]] )
      };
    }
  }
}
</script>

<style lang="sass" scoped>
.chart-resize
  width: 75%
</style>