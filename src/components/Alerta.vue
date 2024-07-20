<script setup>
    const props = defineProps({
        alerta: {
            type: Object,
            required: true
        }
    })
// class="text-white  text-center p-3 uppercase font-bold mb-3 rounded-md bg-red-500" 
</script>

<template>
    <div
    class="p-3 text-center uppercase mb-3"
    :class="[alerta.tipo === 'error' ? 'bg-red-200 border border-red-400 text-white-700 font-semibold' : 'bg-green-200 border border-green-400 text-white-700 font-semibold', 'px-4 py-3 rounded-md relative']"
  >
    {{ alerta.mensaje }}
  </div>
</template>
