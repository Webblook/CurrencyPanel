<template>
  <transition name="modal">

    <div class="modal__mask">
      <div class="modal__container-widgets" v-on:click.stop>
        <h2>Widgets</h2>
        <div class="hr"></div>       
        <div class="container">
          <div class="row">

            <div class="modal__container-widgets-toggle col-12"
            v-for="toggle in toggles"
            v-bind:key="toggle.id">
              <div class="row">
                <div class="col-6 align-self-center">
                  <p>{{ toggle.label }}</p>
                </div>
                <div class="col-6 align-self-center d-flex justify-content-end">
                  <label class="toggle">
                    <input type="checkbox" v-bind:id="toggle.id" v-bind:checked="toggle.isChecked">
                    <span class="slider"></span>
                  </label>
                </div>
              </div>
            </div>

            <div class="modal__container-widgets-confirmation col-12 d-flex justify-content-center">
              <button v-on:click="$emit('selectedToggles', getSelectedToggles())">Save changes</button>
            </div>

          </div>
        </div>
      
      </div>
    </div>

  </transition>
</template>

<script>
  export default {
    data() {
      return {
        labels: ['Favorite currency', 'Selected currency', 'Currency switcher'],
        id: ['favoriteCurrency', 'selectedCurrency', 'currencySwitcher'],
        toggles: []
      }
    },

    mounted() {
      this.setToggles()
    },

    methods: {
      setToggles() {
        // Добавляем в toggles объекты с данными каждого переключателя
        for (let i = 0; i < this.labels.length; i++) {
          this.toggles.push(
            {
              id: this.id[i],
              label: this.labels[i],
              isChecked: true
            }
          );
        };
      },

      getSelectedToggles() {
        // Если id.checked = true, присваиваем toggles[i].isChecked = true
        // Возвращаем массив переключателей
        for (let i = 0; i < this.labels.length; i++) {
          if (document.getElementById(this.id[i]).checked) {
            this.toggles[i].isChecked = true;
          } else {
            this.toggles[i].isChecked = false;
          };
        };
        return this.toggles;
      }
    }
  }
</script>

<style lang="sass" scoped>
@import '../../assets/style/variables.sass'

.modal__container-widgets-toggle
  padding: 2.5rem 1.5rem

p
  font-size: 1.7rem

.toggle
  position: relative
  display: inline-block
  width: 60px
  height: 34px

.toggle input 
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
  border-radius: 34px
  &:before
    border-radius: 50%

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