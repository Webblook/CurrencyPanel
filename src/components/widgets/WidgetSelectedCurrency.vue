<template>
  <div class="col-xl-4 col-lg-6 col-md-6 col-sm-12">
    <div class="widgets-container__item">
      <div class="container-fluid">
        <div class="row">
          <div class="col-6 align-self-center">
           <i class="widgets-container__icon ion-md-cash"></i>
          </div>
          <div class="col-6 align-self-center">
            <h1 class="widgets-container__title">{{ this.dataCurrencies.activeRates[6].toFixed(3) }}</h1>
            <p class="widgets-container__description">EUR to {{ this.selectedCurrency }}</p>
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