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
                  v-on:selectedCurrency="currency => this.selectedCurrency = currency"></widget-currency-switcher>

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
    v-on:selectedChart="chart => { 
      this.currentChartComponent = chart; 
      this.activeModals.showCharts = false 
    }"></modal-charts>

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
      // Добавляем последние 7 дат
      // Формат YYYY-MM-DD
      for (let i = 0; i < 7; i++) {
        let date = new Date();

        date.setDate( date.getDate()-i );
        this.date.years.unshift( date.getFullYear() );
        this.date.months.unshift( ('0' + (date.getMonth()+1)).slice(-2) );
        this.date.dates.unshift( ('0' + date.getDate()).slice(-2) );
      };
    },

    getDataCurrencies() {
      let dates = [];
      let localStorageDates = [];

      // Записываем актуальные даты в формате YYYY-MM-DD
      // Записываем все имеющиеся даты из localStorage
      for (let i = 0; i < 7; i++) {
        dates.push(this.date.years[i]+'-'+this.date.months[i]+'-'+this.date.dates[i]);
        localStorageDates.push(localStorage.key(i));
      }

      // Добавляем все недостающиее и устаревшие даты
      let missingData = dates.filter(number => localStorageDates.indexOf(number) === -1);
      let obsoleteData = localStorageDates.filter(number => dates.indexOf(number) === -1);

      // Удаляем все устаревшие ключи из хранилища, делаем запросы на новые
      // 32400000ms = 9h
      if (Date.now() > Date.UTC(this.date.years[6],this.date.months[6]-1,this.date.dates[6]) + 32400000) {
        for (let i = 0; i < missingData.length; i++) {
          localStorage.removeItem(obsoleteData[i]);
  
          fetch('http://data.fixer.io/api/'+missingData[i]+'?access_key=e9f35012208415f1b93462f7d7943f2a')
          .then(response => response.json())
          .then(response => {
            if (response.success != true) {
              alert('Oops! ' + response.error.info);
            } else {
              localStorage.setItem(missingData[i], JSON.stringify(response));
            };
          })
        };
      };
    },

    setDataCurrencies() {
      // Записываем в dataCurrencies полученные даные
      // Записываем первоначальный курс для прорисовки диаграммы
      for (let i = 0; i < 7; i++) {
        this.dataCurrencies[this.date.years[i]+this.date.months[i]+this.date.dates[i]] = JSON.parse( localStorage.getItem(this.date.years[i]+'-'+this.date.months[i]+'-'+this.date.dates[i]) );
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
      // Закрываем все модалки
      this.activeModals.showWidgets = false;
      this.activeModals.showCharts = false;
      this.activeModals.showContacts = false;
    }
  }
}
</script>

<style lang='sass'>
@import 'assets/style/variables.sass'
@import 'assets/style/mixins.sass'

main
  padding: 1rem 0

.widgets-container__item
  height: 150px
  margin: 2rem 0
  text-align: center
  border-radius: 1rem
  background: #fff
  box-shadow: 0 0 10px #eee
  .row
    height: 150px

.widgets-container__select
  width: 100%
  height: 40px
  color: $black
  background: #fff
  border: 1px solid #2196F3

.widgets-container__icon
  font-size: 6rem
  color: $blue

.widgets-container__title
  font-size: 3rem
  color: $black

.widgets-container__description
  font-size: 1.5rem
  color: $grey

.chart-container
  padding: 2% 0
  background: #fff
  border-radius: 1rem
  box-shadow: 0 0 10px #eee

.modal-enter
  opacity: 0

.modal-leave-active
  opacity: 0

.modal-mask
  position: fixed
  top: 0
  left: 0
  z-index: 9999
  width: 100%
  height: 100%
  background: rgba(0, 0, 0, .5)
  transition: .3s

.modal-container__widgets
  +modal-window(390px, 510px)

.modal-container__charts
  +modal-window(350px, 350px)

.modal-container__contacts
  +modal-window(390px, 420px)

.modal-container__button
  width: 130px
  height: 35px
  margin: 3rem
  font-size: 1.5rem
  color: $lightBlue
  background: none
  border: 2px solid $lightBlue
  border-radius: 5px
  outline: none
  transition: .3s
  cursor: pointer
  &:hover
    background: $lightBlue
    color: #fff
</style>