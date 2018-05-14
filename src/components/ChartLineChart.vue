<template>
  <div class="chart-resize">
    <canvas ref="lineChart"></canvas>
  </div>    
</template>

<script>
export default {
  props: {
    dataCurrencies: {
      type: Object,
      required: true
    },

    selectedCurrency: {
      type: String,
      required: true
    }
  },

  data() {
    return {
      weekDays: [],
      chart: {}
    }
  },

  mounted() {
    this.getWeekDays(),
    this.drawChart()
  },

  methods: {
    getWeekDays() {
      let weekDays = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      let days = [];
  
      for (let i = 0; i < 7; i++) {
        // Получаем названий дней недели за последние 7 дней
        let date = new Date();

        days.push( date.getDay( date.setDate( date.getDate()-i ) ) );
        this.weekDays.push( weekDays[days[i]] )
      };
    },

    drawChart() {
      // Прорисовка диаграммы
      let ctx = this.$refs.lineChart.getContext('2d');
      let chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: [],
          datasets: [{
            label: this.selectedCurrency,
            backgroundColor: 'transparent',
            borderColor: '#03A9F4',
            data: []
          }]
        }
      });

      // Добавляем в data дефолтные данные
      chart.data.datasets.forEach((dataset) => {
        dataset.data.push( this.dataCurrencies.activeRates.forEach((item)=>dataset.data.unshift(item)) );
      });

      // Добавляем в labels дни недели
      this.weekDays.forEach((item)=>chart.data.labels.unshift(item));
      chart.update();

      this.chart = chart;
    }
  },

  watch: {
    selectedCurrency() {
      // При изменении активной валюты добавляем новые данные
      this.chart.data.datasets.forEach((dataset) => {
        dataset.data = [];
        dataset.data.push( this.dataCurrencies.activeRates.forEach((item)=>dataset.data.unshift(item)) );
        dataset.label = this.selectedCurrency;
      });

      this.chart.update();
    }
  }
}
</script>

<style lang="sass" scoped>
.chart-resize
  width: 75%
</style>