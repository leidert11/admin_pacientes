<template>
  <div class="error-message">
    <slot></slot> 
  </div>
</template>

<script setup>
</script>

<style scoped>
.error-message {
  min-height: 1rem; 
  max-height: 1rem; 
  overflow: hidden;   
}
</style>
