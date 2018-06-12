<template>
  <transition name="modal">

    <div class="modal-mask">
      <div class="modal-container__widgets" v-on:click.stop>

        <h2 class="modal-container__title">Widgets</h2>
        <div class="modal-container__line"></div>       
        <div class="container-fluid">
          <div class="row">

            <div class="modal-container__widgets-toggle col-12"
            v-for="toggle in toggles"
            v-bind:key="toggle.id">
              <div class="row">
                <div class="col-6 align-self-center">
                  <p class="modal-container__widgets__name">{{ toggle.label }}</p>
                </div>
                <div class="col-6 align-self-center d-flex justify-content-end">
                  <label class="modal-container__widgets__toggle">
                    <input
                    class="modal-container__widgets__input"
                    type="checkbox"
                    v-bind:id="toggle.id"
                    v-bind:checked="toggle.isChecked"
                    v-on:change="checkNumber">
                    <span class="modal-container__widgets__slider"></span>
                  </label>
                </div>
              </div>
            </div>

            <div class="col-12 d-flex justify-content-center">
              <button class="modal-container__button" v-on:click="$emit('selectedToggles', getSelectedToggles())">Save changes</button>
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
        labels: ['Favorite currency', 'Selected currency', 'Currency switcher', 'Last update'],
        id: ['favoriteCurrency', 'selectedCurrency', 'currencySwitcher', 'lastUpdate'],
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

        this.toggles[this.labels.length-1].isChecked = false;
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
      },

      checkNumber(toggle) {
        let inputs = document.querySelectorAll('.modal-container__widgets__input:checked');

        if (inputs.length > 3) {
          toggle.target.checked = false;

          toggle.target.nextElementSibling.classList.add('shake');
          setTimeout(()=>{
            toggle.target.nextElementSibling.classList.remove('shake');
          }, 1000);
        } else if (inputs.length < 1) {
          toggle.target.checked = true;

          toggle.target.nextElementSibling.classList.add('shake');
          setTimeout(()=>{
            toggle.target.nextElementSibling.classList.remove('shake');
          }, 1000);
        };
      }
    }
  }
</script>

<style lang="sass" scoped>
@import '../../assets/style/variables.sass'

.shake
  animation: shake .8s

@keyframes shake 
  10%, 90% 
    transform: translate(-1px) 
  20%, 80% 
    transform: translate(1px) 
  30%, 50%, 70% 
    transform: translate(-2px)
  40%, 60% 
    transform: translate(2px)
  
.modal-container__widgets-toggle
  padding: 2.5rem 1.5rem

.modal-container__widgets__name
  font-size: 1.7rem

.modal-container__widgets__toggle
  position: relative
  display: inline-block
  width: 60px
  height: 34px

.modal-container__widgets__input
  display: none

.modal-container__widgets__slider
  position: absolute
  top: 0
  left: 0
  right: 0
  bottom: 0
  background: $grey
  border-radius: 34px
  transition: .4s
  cursor: pointer
  &:before
    border-radius: 50%

.modal-container__widgets__slider:before
  content: ''
  position: absolute
  left: 4px
  bottom: 4px
  height: 26px
  width: 26px
  background: #fff
  transition: .4s

.modal-container__widgets__input:checked + .modal-container__widgets__slider
  background-color: $lightBlue

.modal-container__widgets__input:focus + .modal-container__widgets__slider
  box-shadow: 0 0 1px $lightBlue

.modal-container__widgets__input:checked + .modal-container__widgets__slider:before
  transform: translateX(26px)
</style>