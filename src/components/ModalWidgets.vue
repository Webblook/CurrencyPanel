<template>
  <transition name="modal">

    <div class="modal__mask" v-if="showWidgets">
      <div class="modal__container-widgets" v-on:click.stop>
        <h2>Widgets</h2>
        <div class="hr"></div>       
        <div class="container">
          <div class="row">

            <div class="modal__container-widgets-item col-12">
              <div class="row">
                <div class="col-6 align-self-center">
                  <p>Exchange rate</p>
                </div>
                <div class="col-6 align-self-center d-flex justify-content-end">
                  <label class="switch">
                    <input type="checkbox">
                    <span class="slider round"></span>
                  </label>
                </div>
              </div>
            </div>

            <div class="modal__container-widgets-item col-12">
              <div class="row">
                <div class="col-6 align-self-center">
                  <p>Exchange rate</p>
                </div>
                <div class="col-6 align-self-center d-flex justify-content-end">
                  <label class="switch">
                    <input type="checkbox">
                    <span class="slider round"></span>
                  </label>
                </div>
              </div>
            </div>

            <div class="modal__container-widgets-item col-12">
              <div class="row">
                <div class="col-6 align-self-center">
                  <p>Exchange rate</p>
                </div>
                <div class="col-6 align-self-center d-flex justify-content-end">
                  <label class="switch">
                    <input type="checkbox">
                    <span class="slider round"></span>
                  </label>
                </div>
              </div>
            </div>

            <div class="modal__container-widgets-confirmation col-12 d-flex justify-content-center">
              <button>Save changes</button>
            </div>

          </div>
        </div>
      
      </div>
    </div>

  </transition>
</template>

<script>
export default {
  props: {    
    showWidgets: {
      type: Boolean,
      required: true
    }
  }
}
</script>

<style lang="sass" scoped>
@import '../assets/style/variables.sass'
.modal__container-widgets-item
  padding: 2.5rem 1.5rem

p
  font-size: 1.7rem

.switch 
  position: relative
  display: inline-block
  width: 60px
  height: 34px

.switch input 
  display: none

.slider 
  position: absolute
  cursor: pointer
  top: 0
  left: 0
  right: 0
  bottom: 0
  background-color: $grey
  -webkit-transition: .4s
  transition: .4s

.slider:before 
  position: absolute
  content: ""
  height: 26px
  width: 26px
  left: 4px
  bottom: 4px
  background-color: white
  -webkit-transition: .4s
  transition: .4s

input:checked + .slider 
  background-color: $lightBlue

input:focus + .slider 
  box-shadow: 0 0 1px $lightBlue

input:checked + .slider:before 
  -webkit-transform: translateX(26px)
  -ms-transform: translateX(26px)
  transform: translateX(26px)

.slider.round 
  border-radius: 34px

.slider.round:before 
  border-radius: 50%

button
  width: 130px
  height: 35px
  font-size: 1.5rem
  border: 2px solid $lightBlue
  border-radius: 5px
  background: none
  outline: none
  color: $lightBlue
  cursor: pointer
  transition: .3s
  margin: 3rem
  &:hover
    background: $lightBlue
    color: #fff
</style>