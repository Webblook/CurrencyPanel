<template>
  <div class="col-xl-4 col-lg-6 col-md-6 col-sm-12">
    <div class="widgets-container__item">
      <div class="container-fluid">
        <div class="row justify-content-center">
          <div class="col-5 align-self-center">
            <input class="widgets-container__input" 
            type="text" 
            v-model="value"
            maxlength="11">
            <p class="widgets-container__description">{{ firstCurrency }}</p>
          </div>
  
          <div class="col-1 align-self-center">
            <i class="widgets-container__icon widgets-container__w-currency-converter__icon ion-md-swap" v-on:click="swapCurrencies"></i>
          </div>
  
          <div class="col-5 align-self-center">
            <p class="widgets-container__output">{{ result }}</p>
            <p class="widgets-container__description">{{ secondCurrency }}</p>
          </div>
          
        </div>
      </div>
    </div>
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
      value: '',
      result: '0.000',
      ratio: null,
      firstCurrency: 'EUR',
      secondCurrency: 'RUB',
    }
  },

  watch: {
    selectedCurrency: {
      immediate: true,
      handler() {
        // Задаем соотношение Евро к выбранной валюте
        this.value = '';
        this.secondCurrency = this.selectedCurrency;
        this.ratio = this.dataCurrencies.activeRates[6];
      }
    },

    value(value) {
      // конвертируем валюты
      let result = (value * this.ratio).toFixed(3).toString()

      if (result.length > 12) {
        this.result = Math.round(value * this.ratio)
      } else {
        this.result = result
      }
    }
  },

  methods: {
    swapCurrencies() {
      // Задаем новое соотношение, меняем местами переменные
      this.value = '';

      if (this.firstCurrency === 'EUR') {
        this.ratio = 1/this.dataCurrencies.activeRates[6];
      } else {
        this.ratio = this.dataCurrencies.activeRates[6];
      };

      [this.firstCurrency, this.secondCurrency] = [this.secondCurrency, this.firstCurrency];
    }
  }
}
</script>

<style lang="sass" scoped>
@import '../../assets/style/variables.sass'
  
.widgets-container__w-currency-converter__icon
  margin-left: -1rem
  font-size: 2rem
  color: $blue
  cursor: pointer
</style>