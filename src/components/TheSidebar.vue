<template>
  <nav class="sidebar">
    <ul>
      <li><a href="#">Widgets<ion-icon name="cube"></ion-icon></a></li>
      <li><a href="#">Charts<ion-icon name="pie"></ion-icon></a></li>
      <li><a href="#">Settings<ion-icon name="cog"></ion-icon></a></li>
      <li><a href="#">Contact<ion-icon name="mail"></ion-icon></a></li>
    </ul>
  </nav>
</template>

<style lang="sass" scoped>
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
    padding: 2rem 7rem
    text-decoration: none
    color: #424242
    transition: .4s
    &:hover
      color: #03A9F4
  ion-icon
    position: absolute
    top: 2.2rem
    left: 0
    margin-left: 3rem
</style>