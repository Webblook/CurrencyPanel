<template>
  <nav class="sidebar">
    <ul class="sidebar__nav">

      <li class="sidebar__item" v-on:click.stop>
        <a class="sidebar__link" v-on:click="$emit('showWidgets')">
          <span class="sidebar__text">Widgets</span>
          <i class="sidebar__icon ion-md-cube"></i>
        </a>
      </li>

      <li class="sidebar__item" v-on:click.stop>
        <a class="sidebar__link" v-on:click="$emit('showCharts')">
          <span class="sidebar__text">Charts</span>
          <i class="sidebar__icon ion-md-pie"></i>
        </a>
      </li>

      <li class="sidebar__item" v-on:click.stop>
        <a class="sidebar__link" v-on:click="$emit('showContacts')">
          <span class="sidebar__text">Contact</span>
          <i class="sidebar__icon ion-md-mail"></i>
        </a>
      </li>

    </ul>
  </nav>
</template>

<style lang="sass" scoped>
@import '../assets/style/variables.sass'

.sidebar
  position: fixed
  top: 8rem
  left: 0
  height: calc(100vh - 81px)
  padding: 1.5rem 0 0 0
  background: #fff
  box-shadow: 0 5px 10px #eee
  border-top: 1px solid #eee

.sidebar__item
  font-size: 1.6rem
  list-style: none

.sidebar__link
  position: relative
  display: block
  padding: 2.5rem 7rem
  text-decoration: none
  color: $black
  transition: .4s
  cursor: pointer
  &:hover
    color: $lightBlue

.sidebar__icon
  position: absolute
  top: 2.8rem
  left: 0
  margin-left: 3rem

// Extra small devices (portrait phones, less than 576px)
@media (max-width: 575.98px)
  .sidebar
    position: fixed
    top: 7.7rem
    left: 0
    z-index: 10
    height: 60px
    padding: 0

  .sidebar__nav
    display: flex
    justify-content: space-between
    height: 60px

  .sidebar__link
    padding: 3rem 7rem !important
  
  .sidebar__icon
    top: 2.2rem !important

// Small devices (landscape phones, less than 768px)
@media (max-width: 767.98px) {}

// Medium devices (tablets, less than 992px)
@media (max-width: 991.98px) {}

// Large devices (desktops, less than 1200px)
@media (max-width: 1199.98px) 
  .sidebar__text
    display: none
  
  .sidebar__link
    padding: 4rem 0

  .sidebar__icon
    left: 50%
    transform: translate(-50%)
    margin-left: 0
</style>