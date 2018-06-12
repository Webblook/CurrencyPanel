<template>
  <canvas ref="chart"></canvas>
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
    },

    weekDays: {
      type: Array,
      required: true
    }
  },

  data() {
    return {
      chart: {}
    }
  },

  watch: {
    selectedCurrency() {
      // При изменении активной валюты добавляем новые данные
      this.chart.data.datasets.forEach((dataset) => {
        dataset.data = [];
        dataset.data.push( this.dataCurrencies.activeRates.forEach((item)=>dataset.data.push(item)) );
        dataset.label = this.selectedCurrency;
      });

      this.chart.update();
    }
  },

  mounted() {
    this.drawChart()
  },

  methods: {
    drawChart() {
      // Прорисовка диаграммы
      let ctx = this.$refs.chart.getContext('2d');
      let chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: this.weekDays,
          datasets: [{
              label: this.selectedCurrency,
              backgroundColor: 'rgba(3, 169, 244, 0.1)',
              borderColor: '#03A9F4',
              data: this.dataCurrencies.activeRates,
          }]
        },
        options: {}
      });
  
      this.chart = chart;
    }
  }
}
</script>