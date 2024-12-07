<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.png" />
    <nav>
      <!-- <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/my-cards">My Cards</RouterLink> -->
    </nav>
  </header>

  <RouterView />
</template>

<style scoped></style>
