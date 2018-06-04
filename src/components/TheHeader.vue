<template>
  <header class="d-flex justify-content-center">

    <div class="col-4">
      <div class="navbar__brand">
        <h3>The</h3>
        <h2>Currency</h2>
        <h2>Panel 1.0</h2>
      </div>
    </div>

    <div class="col-8">
      <div class="navbar__user d-flex justify-content-end">
        <ul>
          <li>
            <img src="https://www.freeiconspng.com/uploads/flat-face-icon-23.png" alt="user">
            <p>Yuri Gagarin</p>
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
  background: #fff
  box-shadow: 0 0 10px #eee
  z-index: 1
  padding: 0.6rem 2rem

.navbar__brand
  h3
    font-size: 1.5rem
    color: $blueGrey
    padding-left: 1.5rem
  h2
    font-size: 1.8rem
    color: $blueGrey
    letter-spacing: 0.1rem
    &:last-child
      color: $lightBlue

.navbar__user
  ul
    display: flex
    li
      display: flex
      align-items: center
      list-style: none
      margin: 1.5rem
    a
      text-decoration: none
      color: $black
    p
      display: inline
      margin-left: 1.5rem
      font-size: 1.8rem
      color: $black
    img
      width: 35px
      height: 35px

// Extra small devices (portrait phones, less than 576px)
@media (max-width: 575.98px)
  .navbar__brand
  h3
    font-size: 1.3rem !important
  h2
    font-size: 1.6rem !important
</style>