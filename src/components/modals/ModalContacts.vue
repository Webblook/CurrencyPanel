<template>
  <transition name="modal">

     <div class="modal__mask">
        <div class="modal__container-contacts" v-on:click.stop>
          <h2>Contact us</h2>
          <div class="hr"></div>
          
          <div class="modal__container-contacts__form">
            <form method="POST" action="https://formspree.io/daniil.dubrava@yandex.ru">
              <div class="modal__container-contacts__form-email">
                <span class="email">E-mail</span>
                <input id="email" type="email" name="email" maxlength="35" autocomplete="off" required>
              </div>
              <div class="modal__container-contacts__form-message">
                <span class="message">Message</span>
                <textarea name="message" maxlength="350" required></textarea>
              </div>    
              <button type="submit">Send</button>
            </form>
          </div>
        </div>
      </div>

  </transition>
</template>

<style lang="sass" scoped>
@import '../../assets/style/variables.sass'

form
  display: flex
  flex-direction: column
  align-items: center

.modal__container-contacts__form-email,
.modal__container-contacts__form-message
  margin: 2rem 0
  position: relative

span
  position: absolute
  font-size: 1rem
  text-transform: uppercase

.email
  top: -10%
  left: 0

.message
  top: -20%
  left: 0

input, textarea
  font-family: 'Open Sans', sans-serif

input
  width: 350px
  height: 44px
  outline: none
  border: none
  border-bottom: 2px solid $lightBlue
  color: $black

textarea
  width: 350px
  height: 100px
  outline: none
  resize: none
  border: none
  border-bottom: 2px solid $lightBlue
  color: $black

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