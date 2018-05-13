<template>
  <div class="widget">
    <div class="container-fluid">
      <div class="row">
        <div class="col-6 align-self-center">
          <i class="ion-logo-euro"></i>
        </div>
        <div class="col-6 align-self-center">
          <h1>{{ this.dataCurrencies[this.date.years[0]+'/'+this.date.months[0]+'/'+this.date.dates[0]].rates.USD.toFixed(3) }}</h1>
          <p>EUR to USD</p>
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
  }
}
</script>