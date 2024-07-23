<script setup>
  import { ref,reactive } from 'vue'
  import { uid } from 'uid';
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue';

  const pacientes = ref([])

  const state = reactive({
        id: null,
        nombre: "",
        propietario: "",
        email: "",
        alta: "",
        sintomas: "",
    });

    const limpiarFormulario = () => {
      Object.assign(state, {
        nombre : "",
        propietario : "",
        email : "",
        alta : "",
        sintomas : "",
        id:null
      })
    }

    const guardarPaciente = () => {
      if (state.id) {
        const i = pacientes.value.findIndex(pacienteState => pacienteState.id === state.id)
        pacientes.value[i] = {...state}
      } else {
        pacientes.value.push({...state, id: uid()})
      }
      limpiarFormulario()
    }

    const actualizarPaciente = (id) => {
      const paciente = pacientes.value.filter(paciente => paciente.id === id)[0]

      Object.assign(state, paciente)
    }
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
      @guardar-paciente="guardarPaciente"
      :id="state.id"
      
      />
      
      <div class="md:w-1/2 md:h-screen overflow-y-scroll ">
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>

        <div v-if="pacientes.length > 0">

        <p class="text-lg mt-5 text-center mb-10">
          Informacion de pacientes
          <span class="text-indigo-600 font-bold">Adminitralos</span>
        </p>

        <Paciente 
        v-for="paciente in pacientes"
        :paciente="paciente"
        :key="paciente.id"
        @actualizar-paciente="actualizarPaciente"
        />
      </div>
      <p v-else class="text-xl mt-5 text-center">No hay pacientes <span class="text-indigo-600 font-bold">Adminitralos</span></p>
      </div>
    </div>
  </div>
</template>