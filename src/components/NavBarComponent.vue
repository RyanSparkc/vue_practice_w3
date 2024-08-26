<script setup>
import { RouterLink } from 'vue-router'
</script>

<style lang="scss">
.active {
  font-weight: bold;
  color: #42b983 !important;
}
</style>

<template>
  <nav class="navbar navbar-expand-lg bg-body-tertiary mb-4">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">Vue 3 Composition</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent"
        aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <RouterLink class="nav-link" aria-current="page" to="/week1">week1</RouterLink>
          </li>
          <li class="nav-item">
            <RouterLink class="nav-link" to="/week2/login" exact-active-class="active"> week2
            </RouterLink>
          </li>
          <li class="nav-item">
            <RouterLink class="nav-link" to="/week3"> week3</RouterLink>
          </li>

        </ul>
        <div>
          文字測試
        </div>
      </div>
    </div>
  </nav>
</template>
