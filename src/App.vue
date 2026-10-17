<script setup lang="ts">
</script>

<template>
  <h1>Vue 3 + swrv 範例</h1>
  <nav>
    <RouterLink to="/">首頁</RouterLink>
    <RouterLink to="/users">Users API</RouterLink>
  </nav>
  <RouterView />
</template>

<style scoped>
nav {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 1.5rem;
}
</style>
