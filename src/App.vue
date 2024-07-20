<script setup>
  import { ref,reactive } from 'vue'
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'

  const pacientes = ref([])

  const state = reactive({
        nombre: "",
        propietario: "",
        email: "",
        alta: "",
        sintomas: "",
    });

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

    <div class="mt-12 md:flex" >
      <Formulario 
      v-model:nombre="state.nombre"
      v-model:propietario="state.propietario"
      v-model:email="state.email"
      v-model:alta="state.alta"
      v-model:sintomas="state.sintomas"
      
      />
      
      <div class="md:w-1/2 md:h-screen overflow-y-scroll ">
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>

        <div
        v-if="pacientes.length > 0"
        >
      </div>
      <p v-else class="text-xl mt-5 text-center">No hay pacientes <span class="text-indigo-600 font-bold">registralos</span></p>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
