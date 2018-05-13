<template>
  <div class="widget">
    <div class="container-fluid">
      <div class="row justify-content-center">
        <div class="col-5 align-self-center">
          <select>
            <option>EUR</option>
          </select>
        </div>

        <div class="col-1 align-self-center">
          <ion-icon name="arrow-round-forward"></ion-icon>
        </div>

        <div class="col-5 align-self-center">
          <select 
          ref="currencies" 
          v-on:change="$emit('selectedCurrency', getSelectValue())">
            <option selected="selected">RUB</option>
            <option>USD</option>
            <option 
            v-for="(value, key) in this.dataCurrencies[this.date.years[0]+'/'+this.date.months[0]+'/'+this.date.dates[0]].rates" 
            v-bind:key="value.id"
            v-if="key != 'RUB' && key != 'USD'">{{ key }}</option>
          </select>
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
$dimBlack: #424242

select
  width: 100%
  height: 40px
  background: #fff
  border: 1px solid #2196F3
  color: $dimBlack
  
ion-icon
  font-size: 2rem
  margin-left: -1rem
  color: #2196F3
</style>