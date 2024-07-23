<!-- ErrorMessage.vue -->
<template>
    <div class="error-message">
      <p v-if="message" class="text-red-600 text-sm">{{ message }}</p>
    </div>
  </template>
  
  <script setup>
  const props = defineProps({
    message: {
      type: String,
      required: false,
      default: ''
    }
  })
  </script>
  
  <style scoped>
  .error-message {
    min-height: 1.3rem; /* Aumenta esto según sea necesario */
    margin-top: 0.25rem; /* Espacio entre el input y el mensaje de error */
  }
  </style>
  