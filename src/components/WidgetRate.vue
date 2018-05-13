<template>
  <div class="widget d-flex">
    <div class="widget_left align-self-center">
      <ion-icon name="cash"></ion-icon>
    </div>
    <div class="widget_right align-self-center">
      <h1>{{ this.rates[0].toFixed(3) }}</h1>
      <p>EUR to {{ this.selectedCurrency }}</p>
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
    },

    rates: {
      type: Array,
      required: true
    }
  }
}
</script>