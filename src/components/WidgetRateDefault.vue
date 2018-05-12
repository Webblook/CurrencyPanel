<template>
  <div class="widget d-flex">
    <div class="widget_left align-self-center">
      <ion-icon name="logo-euro"></ion-icon>
    </div>
    <div class="widget_right align-self-center">
      <h1>{{ this.dataCurrencies[this.date.years[0]+'/'+this.date.months[0]+'/'+this.date.dates[0]].rates.USD }}</h1>
      <p>EUR to USD</p>
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