<script setup>
  import { ref, reactive, watch, onMounted } from 'vue'
  import { uid } from 'uid'
  import Swal from 'sweetalert2'
  import 'sweetalert2/dist/sweetalert2.min.css';
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue'

  const pacientes = ref([])

  const state = reactive({
    id: null,
    nombre: "",
    propietario: "",
    email: "",
    alta: "",
    sintomas: "",
  })

  const limpiarFormulario = () => {
    Object.assign(state, {
      nombre: "",
      propietario: "",
      email: "",
      alta: "",
      sintomas: "",
      id: null
    })
  }

  const guardarPaciente = () => {
    if (state.id) {
      const i = pacientes.value.findIndex(pacienteState => pacienteState.id === state.id)
      pacientes.value[i] = { ...state }
    } else {
      pacientes.value.push({ ...state, id: uid() })
    }
    limpiarFormulario()
  }

  const actualizarPaciente = (id) => {
    const pacienteEditar = pacientes.value.find(paciente => paciente.id === id)
    Object.assign(state, pacienteEditar)
  }

  const eliminarPaciente = (id) => {
    const index = pacientes.value.findIndex(paciente => paciente.id === id)
    if (index !== -1) {
      pacientes.value.splice(index, 1)
      Swal.fire({
        icon: 'success',
        title: '<span class="font-bold text-green-600">Éxito</span>',
        html: '<p class="font-bold text-green-600 text-2xl">Paciente eliminado correctamente</p>',
      })
    }
  }

  const guardarEnLocalStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }

  watch(pacientes, guardarEnLocalStorage, { deep: true })

  const cargarDesdeLocalStorage = () => {
    const pacientesGuardados = localStorage.getItem('pacientes')
    if (pacientesGuardados) {
      try {
        pacientes.value = JSON.parse(pacientesGuardados)
      } catch (error) {
        console.error('Error al cargar pacientes desde localStorage', error)
      }
    }
  }

  onMounted(cargarDesdeLocalStorage)
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario 
        v-model:nombre="state.nombre"
        v-model:propietario="state.propietario"
        v-model:email="state.email"
        v-model:alta="state.alta"
        v-model:sintomas="state.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="state.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de pacientes
            <span class="text-indigo-600 font-bold">Adminístralos</span>
          </p>
          <Paciente 
            v-for="paciente in pacientes"
            :paciente="paciente"
            :key="paciente.id"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>
        <p v-else class="text-xl mt-5 text-center">No hay pacientes <span class="text-indigo-600 font-bold">Adminístralos</span></p>
      </div>
    </div>
  </div>
</template>
