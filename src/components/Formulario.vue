<script setup>
  import Swal from 'sweetalert2';
  import 'sweetalert2/dist/sweetalert2.min.css';

  const emit = defineEmits([
    'update:nombre',
    'update:propietario',
    'update:email',
    'update:alta',
    'update:sintomas',
    'guardar-paciente'
  ]);

  const props = defineProps({
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
  });

  const validar = () => {
  if (Object.values(props).includes("")) {
    Swal.fire({
      icon: "error",
      title: '<span class="font-bold text-red-600">Error</span>',
      html: '<p class="font-bold text-red-600">Todos los campos son obligatorios</p>'
    });
  } else {
    Swal.fire({
      icon: "success",
      title: '<span class="font-bold text-green-600">Éxito</span>',
      html: '<p class="font-bold text-green-600">Guardado exitosamente</p>'
    });
    emit("guardar-paciente");
  }
};

</script>



<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento de pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Añade pacientes
      <span class="text-indigo-600 font-bold">Adminístralos</span>
    </p>

    <form 
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validar"
    >
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
        />
      </div>
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
        />
      </div>

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
        />
      </div>

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
        />
      </div>

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
        ></textarea>
      </div>

      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold rounded-md hover:bg-indigo-700 cursor-pointer transition-colors"
        value="Registrar paciente"
      />
    </form>
  </div>
</template>
