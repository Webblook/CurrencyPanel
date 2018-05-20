<template>
  <transition name="modal">
     <div class="modal__mask" v-if="showModal">
        <div class="modal__charts" v-on:click.stop>
          <h2>Select the chart</h2>
          <div class="hr"></div>
          <div class="container-fluid">
            <div class="row">
              <div class="col-6 align-self-center">
                <ul>
                  <li><h3>Bar chart</h3></li>
                  <li><h3>Line chart</h3></li>
                </ul>
              </div>
              <div class="col-6 align-self-center">
                <ul>
                  <li>
                    <a><button v-on:click="$emit('bar')">Select</button></a>
                  </li>
                  <li>
                    <a><button v-on:click="$emit('line')">Select</button></a>
                  </li>
                </ul>
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
.modal__mask
  width: 100%
  height: 100%
  position: fixed
  top: 0
  left: 0
  background: rgba(0, 0, 0, .5);
  z-index: 10
  transition: .3s
  text-align: center

.modal__charts
  width: 400px
  height: 400px
  position: fixed
  top: 50%
  left: 50%
  background: #fff
  transform: translate(-50%, -50%)
  z-index: 10
  border-radius: 1rem

h2
  margin: 1rem 2rem
  text-align: left

.hr
  width: 100%
  border: 1px solid #eee

li
  list-style: none
  padding: 2.5rem 0

h2, h3
  font-weight: normal

button
  width: 150px
  height: 25px
  background: #03A9F4
  border: none
  color: #fff
  cursor: pointer
  border-radius: 0.3rem
  outline: none
  transition: .3s
  &:hover
    opacity: .8
</style>