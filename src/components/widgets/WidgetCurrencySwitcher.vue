<template>
  <div class="col-xl-4 col-lg-6 col-md-6 col-sm-12">
    <div class="widget">
      <div class="container-fluid">
        <div class="row justify-content-center">
          <div class="col-5 align-self-center">
            <select>
              <option>EUR</option>
            </select>
          </div>
  
          <div class="col-1 align-self-center">
            <i class="ion-md-arrow-round-forward"></i>
          </div>
  
          <div class="col-5 align-self-center">
            <select 
            ref="currencies" 
            v-on:change="$emit('selectedCurrency', getSelectValue())">
              <option selected="selected">RUB</option>
              <option>USD</option>
              <option 
              v-for="(value, key) in this.dataCurrencies[this.date.years[0]+this.date.months[0]+this.date.dates[0]].rates" 
              v-bind:key="key"
              v-if="key != 'RUB' && key != 'USD'">{{ key }}</option>
            </select>
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

    date: {
      type: Object,
      required: true
    }
  },

  methods: {
    getSelectValue() {
      // Получаем название выбранной валюты
      let select = this.$refs.currencies;
      return select.options[select.selectedIndex].text;
    }
  }
}
</script>

<style lang="sass" scoped>
@import '../../assets/style/variables.sass'

select
  width: 100%
  height: 40px
  color: $black
  background: #fff
  border: 1px solid #2196F3
  
i
  margin-left: -1rem
  font-size: 2rem
  color: $blue
</style>