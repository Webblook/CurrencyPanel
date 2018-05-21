<template>
  <transition name="modal">
     <div class="modal__mask" v-if="showModal">
        <div class="modal__charts" v-on:click.stop>
          <h2>Select a chart</h2>
          <div class="hr"></div>
          <ul>
            <li><a v-on:click="$emit('line')">Line chart</a></li>
            <li><a v-on:click="$emit('bar')">Bar chart</a></li>
            <li><a v-on:click="$emit('radar')">Radar chart</a></li>
            <li><a v-on:click="$emit('horizontalBar')">Horizontal bar chart</a></li>
            <li><a v-on:click="$emit('pie')">Pie chart</a></li>
          </ul>
        </div>
      </div>
  </transition>
</template>

<script>
export default {
  props: {    
    showModal: {
      type: Boolean,
      required: true
    }
  },

  data() {
    return {
      charts: []
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
  width: 400px
  height: 400px
  position: fixed
  top: 50%
  left: 50%
  background: #fff
  transform: translate(-50%, -50%)
  z-index: 9999

h2
  font-weight: normal
  margin: 1rem 1rem
  text-align: left

.hr
  width: 100%
  border: 1px solid #eee

ul
  display: flex
  flex-wrap: wrap

li
  list-style: none
  margin: 1.5rem 1rem
  transition: .3s
  &:hover
    background: $lightBlue

a
  display: block
  text-decoration: none
  cursor: pointer
  padding: 1rem
  border: 2px solid $lightBlue
  color: $blue
  transition: .3s
  &:hover
    color: #fff
</style>