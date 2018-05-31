<template>
  <transition name="modal">

     <div class="modal__mask" v-if="showCharts">
        <div class="modal__container-charts" v-on:click.stop>
          <h2>Chart</h2>
          <div class="hr"></div>

          <div class="modal__container-charts-selection">
            <ul>
              <li 
              v-for="input in inputs"
              v-bind:key="input.id">
                <input type="radio" v-bind:id="input.id" name="charts" v-bind:checked="input.isChecked">
                <label v-bind:for="input.id">{{ input.label }}</label>
              </li>      
            </ul>
          </div>

          <!-- <div class="hr"></div>
          <div class="modal__container-charts-settings d-flex flex-column">
            <div class="modal__container-charts-settings_first align-self-center">
              <p>Background-color</p>
              <input type="text" value="#03A9F4" disabled>
            </div>
            <div class="modal__container-charts-settings_second align-self-center">
              <p>Border-color</p>
              <input type="text" value="#03A9F4" disabled>    
            </div>
          </div>
          <div class="hr"></div> -->

          <div class="modal__container-charts-confirmation d-flex justify-content-center">
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
ul
  display: flex
  flex-wrap: wrap

li
  list-style: none
  margin: 3rem 0.3rem

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
  border-radius: 5px
  color: #757575

.modal__container-charts-settings_first,
.modal__container-charts-settings__second
  margin: 0.5rem

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