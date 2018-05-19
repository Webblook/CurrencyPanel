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
    }
  },

  data() {
    return {
      chart: {},
      weekDays: []
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
      let ctx = this.$refs.chart.getContext('2d');
      let chart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: this.weekDays,
          datasets: [{
              label: this.selectedCurrency,
              backgroundColor: '#03A9F4',
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