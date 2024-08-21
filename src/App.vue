<script setup>
import { RouterView } from 'vue-router'
import NavBarComponent from './components/NavBarComponent.vue'

</script>

<template>
  <header>
    <NavBarComponent />
    <div class="wrapper">
      <!-- <nav>
        <RouterLink to="/">Home</RouterLink> |
        <RouterLink to="/about">About</RouterLink> |
        <RouterLink to="/review">複習</RouterLink> |
        <RouterLink to="/async">非同步</RouterLink> |
        <RouterLink to="/todo">ToDo</RouterLink> |
        <RouterLink to="/week1">練習一</RouterLink> |
        <RouterLink to="/week2">練習二</RouterLink> |
        <RouterLink to="/week3">練習三</RouterLink>
      </nav> -->

    </div>
  </header>
  <div class="container">
    <RouterView />
  </div>

</template>

<style scoped></style>
