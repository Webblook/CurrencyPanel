<template>
  <div class="col-xl-4 col-lg-6 col-md-6 col-sm-12">
    <div class="widgets-container__item">
      <div class="container-fluid">
        <div class="row">
          <div class="col-6 align-self-center">
            <i class="widgets-container__icon ion-md-time"></i>
          </div>
          <div class="col-6 align-self-center">
            <h1 class="widgets-container__title widgets-container__title_size_s">{{ this.date.years[6]+'/'+this.date.months[6]+'/'+this.date.dates[6] }}</h1>
            <p class="widgets-container__description">Last update</p>
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
  }
}
</script>

<style lang='sass' scoped>
.widgets-container__title_size_s
  font-size: 2.5rem
</style>