<template>
  <div id="app">
    <the-header></the-header>

    <div class="container-fluid">
      <div class="row">

        <the-sidebar class="col-xl-2 col-sm-1"></the-sidebar>

        <main class="col-xl-10 offset-xl-2 col-sm-11 offset-sm-1">
          <div class="row flex-column align-items-center">
            <div class="col-12">
              <div class="widgets-container">
                <div class="row justify-content-between">

                  <div class="col-xl-4 col-lg-6 col-md-6 col-sm-12">
                    <widget-rate-default
                    v-bind:dataCurrencies="this.dataCurrencies"
                    v-bind:date="this.date"></widget-rate-default>
                  </div>

                  <div class="col-xl-4 col-lg-6 col-md-6 col-sm-12">
                    <widget-rate
                    v-bind:dataCurrencies="this.dataCurrencies"
                    v-bind:selectedCurrency="this.selectedCurrency"
                    v-bind:rates="this.rates"></widget-rate>
                  </div>

                  <div class="col-xl-4 col-lg-6 col-md-6 col-sm-12">
                    <widget-switcher
                    v-bind:dataCurrencies="this.dataCurrencies"
                    v-bind:date="this.date"
                    v-on:selectedCurrency="setSelectedCurrency"></widget-switcher>
                  </div>

                </div>
              </div>
            </div>

            <div class="col-12">
              <div class="chart-container d-flex justify-content-center">
                <chart-line-chart
                v-bind:dataCurrencies="this.dataCurrencies"
                v-bind:selectedCurrency="this.selectedCurrency"
                v-bind:rates="this.rates"></chart-line-chart> 
              </div>
            </div>

          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script>
import TheHeader from './components/TheHeader.vue';
import TheSidebar from './components/TheSidebar.vue';
import WidgetRate from './components/WidgetRate.vue';
import WidgetRateDefault from './components/WidgetRateDefault.vue';
import WidgetSwitcher from './components/WidgetSwitcher.vue';
import ChartLineChart from './components/ChartLineChart.vue';

export default {
  components: {
    'the-header': TheHeader,
    'the-sidebar': TheSidebar,
    'widget-rate': WidgetRate,
    'widget-rate-default': WidgetRateDefault,
    'widget-switcher': WidgetSwitcher,
    'chart-line-chart': ChartLineChart
  },

  data() {
    return {
      date: {
        years: [],
        months: [],
        dates: []
      },
      dataCurrencies: {},
      selectedCurrency: 'RUB',
      rates: []
    }
  },

  created() {
    this.setDate(),
    this.getDataCurrencies(),
    this.setDataCurrencies(),
    this.setExchangeRate()
  },

  methods: {
    setDate() {
      // Добавляем последние 7 чисел
      // YYYY/MM/DD
      for (let i = 0; i < 7; i++) {
        let date = new Date();

        date.setDate( date.getDate()-i );
        this.date.years.push( date.getFullYear() );
        this.date.months.push( ('0' + (date.getMonth()+1)).slice(-2) );
        this.date.dates.push( ('0' + date.getDate()).slice(-2) );
      };
    },

    getDataCurrencies() {
      // Обновляем курсы валют + устанавливаем новое время в localStorage, если прошло более 24-х
      if (Date.now() > localStorage.getItem('remainingTime')) {
        localStorage.clear();

        let date = new Date();
        let remainingTime = date.getTime( date.setHours(24,0,0,0) ); // Устанавливаем полночь след. дня
        localStorage.setItem('remainingTime', remainingTime);

        // Делаем 7 запросов для получения курса на каждый день
        // Добавляем в localStorage полученные данные
        for (let i = 0; i < 7; i++) {
          fetch('http://data.fixer.io/api/'+this.date.years[i]+'-'+this.date.months[i]+'-'+this.date.dates[i]+'-'+'?access_key=e9f35012208415f1b93462f7d7943f2a')
            .then( response => response.json() )
            .then( response => localStorage.setItem(this.date.years[i]+this.date.months[i]+this.date.dates[i], JSON.stringify(response)) )
        };
      };
    },

    setDataCurrencies() {
      // Записываем в dataCurrencies все полученные данные
      for (let i = 0; i < 7; i++) {
        this.dataCurrencies[this.date.years[i]+this.date.months[i]+this.date.dates[i]] = JSON.parse( localStorage.getItem(this.date.years[i]+this.date.months[i]+this.date.dates[i]) );
      };
    },

    setSelectedCurrency(currency) {
      this.selectedCurrency = currency;
    },

    setExchangeRate() {
      // Добавляем курс для первоначальной прорисовки диаграммы
      for (let i = 0; i < 7; i++) {
        this.rates.push(
          this.dataCurrencies[this.date.years[i]+this.date.months[i]+this.date.dates[i]].rates[this.selectedCurrency]
        );
      };
    }
  },

  watch: {
    selectedCurrency() {
      // Следим за selectedCurrency для перерисовки диаграммы
      this.rates = [];

      for (let i = 0; i < 7; i++) {
        this.rates.push(
          this.dataCurrencies[this.date.years[i]+this.date.months[i]+this.date.dates[i]].rates[this.selectedCurrency]
        );
      };
    }
  }
}
</script>

<style lang='sass'>
$dimBlack: #424242
$dimGrey: #9E9E9E

main
  padding-top: 1rem

.widget
  height: 150px
  background: #fff
  border-radius: 1rem
  box-shadow: 0 0 10px #eee
  margin: 2rem 0
  text-align: center
  i
    font-size: 6rem
    color: #2196F3
  h1
    font-size: 3rem
    color: $dimBlack
  p
    font-size: 1.5rem
    color: $dimGrey
  .row
    height: 150px

.chart-container
  padding: 2% 0
  background: #fff
  border-radius: 1rem
  box-shadow: 0 0 10px #eee
</style>