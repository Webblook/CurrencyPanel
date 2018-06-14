<template>
  <transition name="modal">

    <div class="modal-mask">
      <div class="modal-container__widgets" v-on:click.stop>

        <h2 class="modal-container__title">Widgets</h2>
        <div class="modal-container__line"></div>       
        <div class="container-fluid">
          <div class="row">

            <div class="modal-container__widgets-toggle col-6"
            v-for="toggle in toggles"
            v-bind:key="toggle.id">
              <div class="row">

                <div class="col-6 align-self-center">
                  <p class="modal-container__widgets__name">{{ toggle.label }}</p>
                </div>

                <div class="col-6 align-self-center d-flex justify-content-center">
                  <input 
                  type="checkbox" 
                  class="modal-container__widgets__input"
                  v-bind:id="toggle.id"
                  v-bind:checked="toggle.isChecked"
                  v-on:change="checkNumber">
                  <label class="modal-container__widgets__label" v-bind:for="toggle.id"></label>
                </div>

              </div>
            </div>

            <div class="col-12 d-flex justify-content-center">
              <button class="modal-container__button modal-container__button_m_sm" v-on:click="$emit('selectedToggles', getSelectedToggles())">Save changes</button>
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
        labels: ['Favorite currency', 'Selected currency', 'Currency switcher', 'Last update', 'Currency converter'],
        id: ['favoriteCurrency', 'selectedCurrency', 'currencySwitcher', 'lastUpdate', 'currencyConverter'],
        toggles: []
      }
    },

    mounted() {
      this.setToggles()
    },

    methods: {
      setToggles() {
        // Добавляем в toggles объекты с данными каждого переключателя
        // Первые три по умолчанию - true, все остальные false
        for (let i = 0; i < 3; i++) {
          this.toggles.push(
            {
              id: this.id[i],
              label: this.labels[i],
              isChecked: true
            }
          );
        };

        for (let i = 3; i < this.labels.length; i++) {
          this.toggles.push(
            {
              id: this.id[i],
              label: this.labels[i],
              isChecked: false
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
      },

      checkNumber(toggle) {
        // Устанавливаем маскимальное (3) и минмиальное (1) количества для выбранных виджетов.
        // Добавляем класс с анимацией, при привышении количества
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

.modal-container__widgets__input
  display: none
  &:checked
    left: calc(100% - 5px)
    transform: translateX(-100%)
  &:checked + label
    background: $lightBlue
    
.modal-container__widgets__label
  position: relative
  display: block
  width: 50px
  height: 30px
  background: $grey
  border-radius: 2rem
  transition: .5s
  cursor: pointer
  &:after 
    content: ''
    position: absolute
    top: 5px
    left: 5px
    width: 20px
    height: 20px
    background: #fff
    border-radius: 2rem
    transition: 0.3s

input:checked + label:after 
  left: calc(100% - 5px)
  transform: translateX(-100%)

// Extra small devices (portrait phones, less than 576px)
@media (max-width: 575.98px) 
  .modal-container__widgets__label
    left: 1.5rem
</style>