<script setup>

    //cuando es un solo dato es bueno utilizar ref es simple pero cuando
    // son multiples datos como en este caso es mejor utilizar reacctive ya que es un objeto
    
    // import { ref } from 'vue'

    // const nombre = ref('')
    
    import { reactive } from "vue";
    import  Alerta  from './Alerta.vue'

    const alerta = reactive({
      tipo: "",
      mensaje: ""   
    })

    const emit = defineEmits([
      'update:nombre',
      'update:propietario',
      'update:email',
      'update:alta',
      'update:sintomas',
      'guardar-paciente'
    ])

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
    })
    const validar = () => {
      Object.values(props).includes("")
        ? (alerta.mensaje = "Todos los campos son obligatorios", alerta.tipo = "error")
        : (alerta.mensaje = "Guardado exitosamente", alerta.tipo = "success");

        emit("guardar-paciente")
    };
    
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento de pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      añade pacientes
      <span class="text-indigo-600 font-bold">Adminitralos</span>
    </p>

    <Alerta 
    v-if="alerta.mensaje" 
    :alerta="alerta"
    />
    <form 
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validar" 
      >
      
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold">
          nombre mascota
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
          nombre del propietario
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
          email del propietario
        </label>
        <input
          id="email"
          type="email"
          placeholder="email del propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-gray-700 uppercase font-bold">
          alta
        </label>
        <input
          id="alta"
          type="date"
          placeholder="alta"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold">
          sintomas
        </label>
        <textarea
          id="sintomas"
          placeholder="sintomas"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-20"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        ></textarea>
      </div>

      <input
        type="submit"
        class="bg-indigo-600 w-full p-3 text-white uppercase font-bold rounded-md hover:bg-indigo-700 cursor-pointer transition-colors"
        value="registrar paciente"
      />
    </form>
  </div>
</template>
