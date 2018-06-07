<template>
  <div id="app" v-on:click="hideModals()">
    <the-header></the-header>

    <div class="container-fluid">
      <div class="row">

        <the-sidebar class="col-xl-2 col-sm-1" 
        v-on:showWidgets="activeModals.showWidgets = true"
        v-on:showCharts="activeModals.showCharts = true"
        v-on:showContacts="activeModals.showContacts = true"></the-sidebar>
        
        <main class="col-xl-10 offset-xl-2 col-sm-11 offset-sm-1">
          <div class="row">
            <div class="col-12">
              <div class="widgets-container">
                <div class="row">

                  <!-- Favorite currency -->
                  <widget-favorite-currency
                  v-show="activeWidgets.favoriteCurrency"
                  v-bind:dataCurrencies="this.dataCurrencies"
                  v-bind:date="this.date"></widget-favorite-currency>

                  <!-- Selected currency -->
                  <widget-selected-currency
                  v-show="activeWidgets.selectedCurrency"
                  v-bind:dataCurrencies="this.dataCurrencies"
                  v-bind:selectedCurrency="this.selectedCurrency"></widget-selected-currency>

                  <!-- Currency switcher -->
                  <widget-currency-switcher
                  v-show="activeWidgets.currencySwitcher"
                  v-bind:dataCurrencies="this.dataCurrencies"
                  v-bind:date="this.date"
                  v-on:selectedCurrency="(currency) => this.selectedCurrency = currency"></widget-currency-switcher>

                  <!-- Last update -->
                  <widget-last-update
                  v-show="activeWidgets.lastUpdate"
                  v-bind:dataCurrencies="this.dataCurrencies"
                  v-bind:date="this.date"></widget-last-update>

                </div>
              </div>
            </div>

            <div class="col-12"> 
              <div class="chart-container d-flex justify-content-center">
                <chart-base
                v-bind:dataCurrencies="this.dataCurrencies"
                v-bind:selectedCurrency="this.selectedCurrency"
                v-bind:currentChartComponent="this.currentChartComponent"></chart-base>
              </div>
            </div>

          </div>
        </main>
      </div>
    </div>

    <!-- Modals -->
    <modal-widgets
    v-show="activeModals.showWidgets"
    v-on:selectedToggles="setSelectedWidgets"></modal-widgets>

    <modal-charts
    v-show="activeModals.showCharts"
    v-on:selectedChart="(chart) => {this.currentChartComponent = chart; this.activeModals.showCharts = false}"></modal-charts>

    <modal-contacts
    v-show="activeModals.showContacts"></modal-contacts>

  </div>
</template>

<script>
// Single-instance components
import TheHeader from './components/TheHeader.vue';
import TheSidebar from './components/TheSidebar.vue';

// Chart
import ChartBase from './components/charts/ChartBase.vue';

// Modals
import ModalWidgets from './components/modals/ModalWidgets.vue';
import ModalCharts from './components/modals/ModalCharts.vue';
import ModalContacts from './components/modals/ModalContacts.vue';

// Widgets
import WidgetFavoriteCurrency from './components/widgets/WidgetFavoriteCurrency.vue';
import WidgetSelectedCurrency from './components/widgets/WidgetSelectedCurrency.vue';
import WidgetCurrencySwitcher from './components/widgets/WidgetCurrencySwitcher.vue';
import WidgetLastUpdate from './components/widgets/WidgetLastUpdate.vue'

export default {
  components: {
    'the-header': TheHeader,
    'the-sidebar': TheSidebar,
    'chart-base': ChartBase,
    'modal-widgets': ModalWidgets,
    'modal-charts': ModalCharts,
    'modal-contacts': ModalContacts,
    'widget-favorite-currency': WidgetFavoriteCurrency,
    'widget-selected-currency': WidgetSelectedCurrency,
    'widget-currency-switcher': WidgetCurrencySwitcher,
    'widget-last-update': WidgetLastUpdate
  },

  data() {
    return {
      date: {
        years: [],
        months: [],
        dates: []
      },

      dataCurrencies: {
        activeRates: []
      },

      selectedCurrency: 'RUB',
      currentChartComponent: 'chart-line',

      activeWidgets: {
        favoriteCurrency: true,
        selectedCurrency: true,
        currencySwitcher: true,
        lastUpdate: false
      },

      activeModals: {
        showWidgets: false,
        showCharts: false,
        showContacts: false
      }
    }
  },

  watch: {
    selectedCurrency() {
      // Меняем данные в activeRates при изменении выбранной валюты
      this.dataCurrencies.activeRates = [];
      for (let i = 0; i < 7; i++) {
        this.dataCurrencies.activeRates.push( this.dataCurrencies[this.date.years[i]+this.date.months[i]+this.date.dates[i]].rates[this.selectedCurrency] );
      };
    }
  },

  created() {
    this.setDate(),
    this.getDataCurrencies(),
    this.setDataCurrencies()
  },

  methods: {
    setDate() {
      // Добавляем последние 7 чисел
      // YYYY/MM/DD
      for (let i = 0; i < 7; i++) {
        let date = new Date();

        date.setDate( date.getDate()-i );
        this.date.years.unshift( date.getFullYear() );
        this.date.months.unshift( ('0' + (date.getMonth()+1)).slice(-2) );
        this.date.dates.unshift( ('0' + date.getDate()).slice(-2) );
      };
    },

    getDataCurrencies() {
      // Обновляем курсы валют + устанавливаем новое время в localStorage, если прошло более 24-х
      if (Date.now() > localStorage.getItem('remainingTime')) {
        localStorage.clear();

        let date = new Date();
        let remainingTime = date.getTime( date.setHours(24,0,0,0) ); // Устанавливаем полночь след. дня
        localStorage.setItem('remainingTime', remainingTime);

        // Делаем запросы на последние 7 дней, добавляем в localStorage
        for (let i = 0; i < 7; i++) {
          fetch('http://data.fixer.io/api/'+this.date.years[i]+'-'+this.date.months[i]+'-'+this.date.dates[i]+'-'+'?access_key=9715c4c33820dac62fe129e9506ea668')
            .then( response => response.json() )
            .then( response => localStorage.setItem(this.date.years[i]+this.date.months[i]+this.date.dates[i], JSON.stringify(response)) )
        };
      };
    },

    setDataCurrencies() {
      // Записываем в dataCurrencies полученные даные
      // Записываем первоначальный курс для прорисовки диаграммы
      for (let i = 0; i < 7; i++) {
        this.dataCurrencies[this.date.years[i]+this.date.months[i]+this.date.dates[i]] = JSON.parse( localStorage.getItem(this.date.years[i]+this.date.months[i]+this.date.dates[i]) );
        this.dataCurrencies.activeRates.push( this.dataCurrencies[this.date.years[i]+this.date.months[i]+this.date.dates[i]].rates[this.selectedCurrency] );
      };
    },

    setSelectedWidgets(widgets) {
      // Закрываем модалку showWidgets
      // Устанавливаем значения isChecked, полученные из widgets
      this.activeModals.showWidgets = false;

      for (let i = 0; i < widgets.length; i++) {
        this.activeWidgets[widgets[i].id] = widgets[i].isChecked;
      }
    },

    hideModals() {
      // Заркываем все модалки
      this.activeModals.showWidgets = false;
      this.activeModals.showCharts = false;
      this.activeModals.showContacts = false;
    }
  }
}
</script>

<style lang='sass'>
@import 'assets/style/variables.sass'

main
  padding: 1rem 0

.widget
  height: 150px
  margin: 2rem 0
  text-align: center
  border-radius: 1rem
  background: #fff
  box-shadow: 0 0 10px #eee
  i
    font-size: 6rem
    color: #2196F3
  h1
    font-size: 3rem
    color: $black
  p
    font-size: 1.5rem
    color: $grey
  .row
    height: 150px

.chart-container
  padding: 2% 0
  background: #fff
  border-radius: 1rem
  box-shadow: 0 0 10px #eee

.modal-enter 
  opacity: 0

.modal-leave-active 
  opacity: 0

.modal__mask
  position: fixed
  top: 0
  left: 0
  z-index: 9999
  width: 100%
  height: 100%
  background: rgba(0, 0, 0, .5);
  transition: .3s

.modal__container-widgets,
.modal__container-charts,
.modal__container-contacts
  position: fixed
  top: 50%
  left: 50%
  transform: translate(-50%, -50%)
  z-index: 9999
  width: 390px
  height: 440px
  padding: 1.5rem
  background: #fff
  border-radius: 1.5rem
  h2
    margin: 1rem 1rem
    font-weight: normal
    font-size: 2.5rem
  .hr
    width: 100%
    margin: 1rem 0
    border: 0.5px solid #eee

.modal__container-charts
  width: 350px
  height: 350px

.modal__container-contacts
  width: 390px
  height: 420px

.modal__container-widgets
  height: 510px
</style>