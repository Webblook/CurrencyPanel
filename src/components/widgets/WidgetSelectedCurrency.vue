<template>
  <div class="col-xl-4 col-lg-6 col-md-6 col-sm-12">
    <div class="widget">
      <div class="container-fluid">
        <div class="row">
          <div class="col-6 align-self-center">
           <i class="ion-md-cash"></i>
          </div>
          <div class="col-6 align-self-center">
            <h1>{{ this.dataCurrencies.activeRates[6].toFixed(3) }}</h1>
            <p>EUR to {{ this.selectedCurrency }}</p>
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
  }
}
</script>