<template>
  <transition name="modal">

     <div class="modal__mask" v-if="showCharts">
        <div class="modal__charts" v-on:click.stop>
          <h2>Chart settings</h2>
          <div class="hr"></div>

          <div class="modal__chart-selection">
            <ul>
              <li 
              v-for="input in inputs"
              v-bind:key="input.id">
                <input type="radio" v-bind:id="input.id" name="charts" v-bind:checked="input.isChecked">
                <label v-bind:for="input.id">{{ input.label }}</label>
              </li>      
            </ul>
          </div>

          <div class="hr"></div>

          <div class="modal__chart-settings d-flex flex-column">
            <div class="modal__chart-settings_first align-self-center">
              <p>Background-color</p>
              <input type="text" value="#03A9F4" disabled>
            </div>
            <div class="modal__chart-settings_second align-self-center">
              <p>Border-color</p>
              <input type="text" value="#03A9F4" disabled>    
            </div>
          </div>

          <div class="hr"></div>
          <div class="modal__chart-confirmation d-flex justify-content-end">
            <button v-on:click="$emit('selectedChart', getSelectedChart())">Save</button>
          </div>
        </div>
      </div>

  </transition>
</template>

<script>
export default {
  props: {    
    showCharts: {
      type: Boolean,
      required: true
    }
  },

  data() {
    return {
      charts: ['chart-line', 'chart-bar', 'chart-radar', 'chart-horizontal-bar', 'chart-pie'],
      inputs: []
    }
  },

  mounted() {
    this.setInputs()
  },

  methods: {
    setInputs() {
      let labels = ['Line chart', 'Bar chart', 'Radar chart', 'Horizontal bar chart', 'Pie chart'];

      for (let i = 0; i < this.charts.length; i++) {
        this.inputs.push(
          {
            id: this.charts[i],
            label: labels[i],
            isChecked: false
          }
        );
      };

      this.inputs[0].isChecked = true;
    },

    getSelectedChart() {
      // Устанавливаем всем input'ам isChecked = false
      for (let i = 0; i < this.charts.length; i++) { 
        this.inputs[i].isChecked = false;
      };

      // Если input.checked = true, присваиваем isChecked = true
      // Возвращаем выбранный компонент
      for (let i = 0; i < this.charts.length; i++) {
        if (document.getElementById(this.charts[i]).checked) {
          this.inputs[i].isChecked = true;
          return this.charts[i];
        };
      };
    }
  }
}
</script>

<style lang="sass" scoped>
@import '../assets/style/variables.sass'
.modal__mask
  width: 100%
  height: 100%
  position: fixed
  top: 0
  left: 0
  background: rgba(0, 0, 0, .5);
  z-index: 9999
  transition: .3s

.modal__charts
  width: 420px
  height: 430px
  position: fixed
  top: 50%
  left: 50%
  background: #fff
  transform: translate(-50%, -50%)
  z-index: 9999

h2
  font-weight: normal
  font-size: 2.5rem
  margin: 1rem 1rem

.hr
  width: 100%
  border: 0.5px solid #eee
  margin: 1rem 0

ul
  display: flex
  flex-wrap: wrap

li
  list-style: none
  margin: 2rem 1rem

input
  font-family: 'Open Sans', sans-serif
  
input[type=radio]
  display: none

input[type=radio] + label
  cursor: pointer
  padding: 1rem
  border: 2px solid $lightBlue
  color: $blue
  transition: .3s
  &:hover
    color: #fff
    background: $lightBlue

input[type=radio]:checked + label
  color: #fff
  background: $lightBlue

p
  font-size: 1.5rem

input
  border: 1px solid #E0E0E0
  outline: none
  width: 300px
  height: 25px
  margin: 0.5rem 0
  padding: 0 1rem
  border-radius: 5px
  color: #757575

.modal__chart-settings_first,
.modal__chart-settings_second
  margin: 0.5rem

button
  width: 70px
  height: 40px
  border: 2px solid $lightBlue
  background: none
  outline: none
  border-radius: 7px
  color: $lightBlue
  margin: 1rem 2rem
  cursor: pointer
  transition: .3s
  &:hover
    background: $lightBlue
    color: #fff
</style>