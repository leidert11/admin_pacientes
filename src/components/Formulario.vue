<script setup>
import { ref, computed } from 'vue'
import Swal from 'sweetalert2'
import 'sweetalert2/dist/sweetalert2.min.css'
import ErrorMessage from './ErrorMessage.vue'

const emit = defineEmits([
  'update:nombre',
  'update:propietario',
  'update:email',
  'update:alta',
  'update:sintomas',
  'guardar-paciente'
])

const props = defineProps({
  id: {
    type: [String, null],
    required: true
  },
  nombre: {
    type: String,
    required: true
  },
  propietario: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true
  },
  alta: {
    type: String,
    required: true
  },
  sintomas: {
    type: String,
    required: true
  }
})

const errors = ref({})

const validar = () => {
  errors.value = {}

  if (!props.nombre) errors.value.nombre = 'El nombre de la mascota es obligatorio.'
  if (!props.propietario) errors.value.propietario = 'El nombre del propietario es obligatorio.'
  if (!props.email) {
    errors.value.email = 'El email del propietario es obligatorio.'
  } else if (!/\S+@\S+\.\S+/.test(props.email)) {
    errors.value.email = 'El email debe ser válido.'
  }
  if (!props.alta) errors.value.alta = 'La fecha de alta es obligatoria.'
  if (!props.sintomas) errors.value.sintomas = 'Los síntomas son obligatorios.'

  if (Object.keys(errors.value).length === 0) {
    Swal.fire({
      icon: 'success',
      title: '<span class="font-bold text-green-600">Éxito</span>',
      html: '<p class="font-bold text-green-600 text-2xl">Guardado exitosamente</p>',
    }).then(() => {
      emit('guardar-paciente')
    })
  }
}

const editarBoton = computed(() => props.id)
</script>

<template>
  <div class="w-full md:w-1/2 mx-auto">
    <h2 class="font-black text-2xl md:text-3xl text-center">Seguimiento de pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Añade pacientes
      <span class="text-indigo-600 font-bold">Adminístralos</span>
    </p>

    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validar"
    >
      <!-- Nombre mascota -->
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">
          Nombre mascota
        </label>
        <input
          id="mascota"
          type="text"
          placeholder="Nombre de la mascota"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="nombre"
          @input="$emit('update:nombre', $event.target.value)"
          :class="{'border-red-500': errors.nombre}"
        />
        <div class="h-6">
          <ErrorMessage v-if="errors.nombre">
            <p class="text-red-600 text-sm">{{ errors.nombre }}</p>
          </ErrorMessage>
        </div>
      </div>

      <!-- Nombre del propietario -->
      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold">
          Nombre del propietario
        </label>
        <input
          id="propietario"
          type="text"
          placeholder="Nombre del propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
          :class="{'border-red-500': errors.propietario}"
        />
        <div class="h-6">
          <ErrorMessage v-if="errors.propietario">
            <p class="text-red-600 text-sm">{{ errors.propietario }}</p>
          </ErrorMessage>
        </div>
      </div>

      <!-- Email del propietario -->
      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold">
          Email del propietario
        </label>
        <input
          id="email"
          type="email"
          placeholder="Email del propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
          :class="{'border-red-500': errors.email}"
        />
        <div class="h-6">
          <ErrorMessage v-if="errors.email">
            <p class="text-red-600 text-sm">{{ errors.email }}</p>
          </ErrorMessage>
        </div>
      </div>

      <!-- Fecha de alta -->
      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">
          Alta
        </label>
        <input
          id="alta"
          type="date"
          placeholder="Alta"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
          :class="{'border-red-500': errors.alta}"
        />
        <div class="h-6">
          <ErrorMessage v-if="errors.alta">
            <p class="text-red-600 text-sm">{{ errors.alta }}</p>
          </ErrorMessage>
        </div>
      </div>

      <!-- Síntomas -->
      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">
          Síntomas
        </label>
        <textarea
          id="sintomas"
          placeholder="Síntomas"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-20"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
          :class="{'border-red-500': errors.sintomas}"
        ></textarea>
        <div class="h-6">
          <ErrorMessage v-if="errors.sintomas">
            <p class="text-red-600 text-sm">{{ errors.sintomas }}</p>
          </ErrorMessage>
        </div>
      </div>

      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold rounded-md hover:bg-indigo-700 cursor-pointer transition-colors"
        :value="[editarBoton ? 'Guardar Cambios' : 'Registrar Paciente']"
      />
    </form>
  </div>
</template>
