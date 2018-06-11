<template>
  <transition name="modal">

     <div class="modal-mask">
        <div class="modal-container__contacts" v-on:click.stop>

          <h2 class="modal-container__title">Contact us</h2>
          <div class="modal-container__line"></div>

            <form class="modal-container__contacts__form" method="POST" action="https://formspree.io/daniil.dubrava@yandex.ru">

              <div class="modal-container__contacts__form-email">
                <span class="modal-container__contacts__email">E-mail</span>
                <input class="modal-container__contacts__input" id="email" type="email" name="email" maxlength="35" autocomplete="off" required>
              </div>

              <div class="modal-container__contacts__form-message">
                <span class="modal-container__contacts__message">Message</span>
                <textarea class="modal-container__contacts__textarea" name="message" maxlength="350" required></textarea>
              </div>  

              <button class="modal-container__button" type="submit">Send</button>

            </form>

        </div>
      </div>

  </transition>
</template>

<style lang="sass" scoped>
@import '../../assets/style/variables.sass'

.modal-container__contacts__form
  display: flex
  flex-direction: column
  align-items: center

.modal-container__contacts__form-email,
.modal-container__contacts__form-message
  position: relative
  margin: 2rem 0

.modal-container__contacts__email,
.modal-container__contacts__message 
  position: absolute
  font-size: 1rem
  text-transform: uppercase

.modal-container__contacts__email
  top: -10%
  left: 0

.modal-container__contacts__message
  top: -20%
  left: 0

.modal-container__contacts__input,
.modal-container__contacts__textarea
  font-family: 'Open Sans', sans-serif

.modal-container__contacts__input
  width: 350px
  height: 44px
  color: $black
  border: none
  border-bottom: 2px solid $lightBlue
  outline: none
  
.modal-container__contacts__textarea
  width: 350px
  height: 100px
  color: $black
  border: none
  border-bottom: 2px solid $lightBlue
  outline: none
  resize: none
</style>