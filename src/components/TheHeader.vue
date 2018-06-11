<template>
  <header class="d-flex justify-content-center">

    <div class="col-4">
      <div class="header__brand">
        <h3 class="header__title_size_sm">The</h3>
        <h2 class="header__title_size_lg">Currency</h2>
        <h2 class="header__title_size_lg">Panel 1.0</h2>
      </div>
    </div>

    <div class="col-8">
      <div class="header__user d-flex justify-content-end">
        <ul class="header__list">
          <li class="header__item">
            <img class="header__img" src="https://www.freeiconspng.com/uploads/flat-face-icon-23.png" alt="user">
            <p class="header__name">Yuri Gagarin</p>
          </li>
        </ul>      
      </div>
    </div>
    
  </header>
</template>

<style lang="sass" scoped>
@import '../assets/style/variables.sass'

header
  position: fixed
  top: 0
  left: 0
  right: 0
  z-index: 1
  padding: 0.6rem 2rem
  background: #fff
  box-shadow: 0 0 10px #eee

.header__title_size_sm
  padding-left: 1.5rem
  font-size: 1.5rem
  color: $blueGrey

.header__title_size_lg
  font-size: 1.8rem
  letter-spacing: 0.1rem
  color: $blueGrey
  &:last-child
    color: $lightBlue

.header__list
  display: flex

.header__item
  display: flex
  align-items: center
  margin: 1.5rem
  list-style: none

.header__link
  text-decoration: none
  color: $black

.header__name
  display: inline
  margin-left: 1.5rem
  font-size: 1.8rem
  color: $black

.header__img
  width: 35px
  height: 35px

// Extra small devices (portrait phones, less than 576px)
@media (max-width: 575.98px)
  .navbar__brand
    .header__title_size_sm
      font-size: 1.3rem !important
    .header__title_size_lg
      font-size: 1.6rem !important
</style>