<script setup lang="ts">
import Loader from './components/Loader.vue';
</script>

<template>
  <div class="page">
    <Loader />
  </div>
</template>

<style scoped>
.page {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  perspective: 800px;
}
</style>
