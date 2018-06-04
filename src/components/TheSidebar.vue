<template>
  <nav class="sidebar">
    <ul>

      <li v-on:click.stop>
        <a v-on:click="$emit('showWidgets')">
          <span>Widgets</span>
          <i class="ion-md-cube"></i>
        </a>
      </li>

      <li v-on:click.stop>
        <a v-on:click="$emit('showCharts')">
          <span>Charts</span>
          <i class="ion-md-pie"></i>
        </a>
      </li>

      <li v-on:click.stop>
        <a v-on:click="$emit('showContacts')">
          <span>Contact</span>
          <i class="ion-md-mail"></i>
        </a>
      </li>

    </ul>
  </nav>
</template>

<style lang="sass" scoped>
@import '../assets/style/variables.sass'

.sidebar
  height: calc(100vh - 81px)
  position: fixed
  top: 8rem
  left: 0
  background: #fff
  box-shadow: 0 5px 10px #eee
  border-top: 1px solid #eee
  padding: 1.5rem 0 0 0
  li
    font-size: 1.6rem
    list-style: none
  a
    position: relative
    display: block
    padding: 2.5rem 7rem
    text-decoration: none
    color: $black
    transition: .4s
    cursor: pointer
    &:hover
      color: $lightBlue
  i
    position: absolute
    top: 2.8rem
    left: 0
    margin-left: 3rem

// Extra small devices (portrait phones, less than 576px)
@media (max-width: 575.98px)
  .sidebar
    height: 60px
    position: fixed
    top: 7.7rem
    left: 0
    padding: 0
    z-index: 10
  ul
    height: 60px
    display: flex
    justify-content: space-between
  a
    padding: 3rem 7rem !important
  i
    top: 2.2rem !important

// Small devices (landscape phones, less than 768px)
@media (max-width: 767.98px) {}

// Medium devices (tablets, less than 992px)
@media (max-width: 991.98px) {}

// Large devices (desktops, less than 1200px)
@media (max-width: 1199.98px) 
  .sidebar
    span
      display: none
    a
      padding: 4rem 0
    i
      left: 50%
      transform: translate(-50%)
      margin-left: 0
</style>