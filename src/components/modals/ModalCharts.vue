<template>
  <transition name="modal">

    <div class="modal-mask" v-on:click="resetChecked">
      <div class="modal-container__charts" v-on:click.stop>

        <h2 class="modal-container__title">Charts</h2>
        <div class="modal-container__line"></div>

        <div class="modal-container__charts__inputs-container">
          <div class="modal-container__charts__inputs-container__item"
          v-for="input in inputs"
          v-bind:key="input.id">
            <input class="modal-container__charts__input" type="radio" v-bind:id="input.id" name="charts">
            <label class="modal-container__charts__label" v-bind:for="input.id">{{ input.label }}</label>
          </div>
        </div>

        <div class="d-flex justify-content-center">
          <button class="modal-container__button" v-on:click="$emit('selectedChart', getSelectedChart())">Save changes</button>
        </div>
        
      </div>
    </div>

  </transition>
</template>

<script>
export default {
  data() {
    return {
      labels: ['Line chart', 'Bar chart', 'Radar chart', 'Horizontal bar chart', 'Pie chart'],
      id: ['chart-line', 'chart-bar', 'chart-radar', 'chart-horizontal-bar', 'chart-pie'],
      inputs: [],
      currentChartComponent: 'chart-line',
    }
  },

  mounted() {
    this.setInputs()
  },

  methods: {
    setInputs() {
      // Добавляем в inputs объекты с данными каждого input'a
      for (let i = 0; i < this.labels.length; i++) {
        this.inputs.push(
          {
            id: this.id[i],
            label: this.labels[i]
          }
        );
      };

      // Без задержки, постоянно null. Без костыля, не смог решить
      setTimeout(()=>{
        document.getElementById(this.id[0]).checked = true;
      }, 50)
    },

    getSelectedChart() {
      // Если id.checked = true, присваиваем inputs[i].isChecked = true
      // Возвращаем выбранную диаграмму
      for (let i = 0; i < this.labels.length; i++) {
        if (document.getElementById(this.id[i]).checked) {
          this.currentChartComponent = this.id[i];
          return this.id[i]
        };
      };    
    },

    resetChecked() {
      // Перед закрытием, все сбрасываем, кроме выбранного
      for (let i = 0; i < this.labels.length; i++) {
        document.getElementById(this.id[i]).checked = false;
      };

      document.getElementById(this.currentChartComponent).checked = true;
    }
  }
}
</script>

<style lang="sass" scoped>
@import '../../assets/style/variables.sass'

.modal-container__charts__inputs-container
  display: flex
  flex-wrap: wrap

.modal-container__charts__inputs-container__item
  margin: 3rem 0.3rem

.modal-container__charts__input
  font-family: 'Open Sans', sans-serif
  display: none

.modal-container__charts__input + .modal-container__charts__label
  padding: 1rem
  color: $blue
  border: 2px solid $lightBlue
  transition: .3s
  cursor: pointer
  &:hover
    color: #fff
    background: $lightBlue

.modal-container__charts__input:checked + .modal-container__charts__label
  color: #fff
  background: $lightBlue
</style>