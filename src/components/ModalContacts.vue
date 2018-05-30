<template>
  <transition name="modal">

     <div class="modal__mask" v-if="showContacts">
        <div class="modal__contacts" v-on:click.stop>
          <h2>Contacts</h2>
          <div class="hr"></div>
          
          <div class="modal__contacts__form">
            <form method="POST" action="https://formspree.io/daniil.dubrava@yandex.ru">
              <input type="email" name="email" placeholder="Your email" maxlength="35" autocomplete="off" required>
              <textarea name="message" placeholder="Your message" maxlength="350" required></textarea>
              <button type="submit">Send</button>
            </form>
          </div>
        </div>
      </div>

  </transition>
</template>

<script>
export default {
  props: {    
    showContacts: {
      type: Boolean,
      required: true
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

.modal__contacts
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

form
  display: flex
  flex-direction: column
  align-items: center

input, textarea
  font-family: 'Open Sans', sans-serif

input
  width: 350px
  height: 44px
  outline: none
  color: $black
  border: 1px solid $lightBlue
  margin: 2rem
  padding: 0 2rem

textarea
  width: 350px
  height: 200px
  outline: none
  resize: none
  color: $black
  border: 1px solid $lightBlue
  padding: 1rem 2rem

button
  width: 350px
  height: 30px
  background: none
  outline: none
  color: $lightBlue
  border: 1px solid $lightBlue
  border-radius: 7px
  cursor: pointer
  margin: 1rem 2rem
  transition: .3s
  &:hover
    background: $lightBlue
    color: #fff
</style>