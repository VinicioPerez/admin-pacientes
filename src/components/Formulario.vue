<script setup>
import { ref, reactive, computed} from 'vue'
import Alerta from './Alerta.vue';

const alerta = reactive({
    mensaje: '',
    tipo: ''
});

const emit = defineEmits([
    'update:nombre',
    'update:propietario',
    'update:email',
    'update:alta',
    'update:sintomas',
    'guardar-paciente'
]);

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

const validar = () => {
    if (Object.values(props).includes('')) {
        alerta.mensaje = 'Todos los campos son obligatorios'
        alerta.tipo = 'error'
        return
    }
    emit('guardar-paciente')
    alerta.mensaje = 'Paciente Guardado Correctamente'
    alerta.tipo= 'exito'

    setTimeout(() => {
        Object.assign(alerta, {
            mensaje: '',
            tipo: ''
        })        
    }, 3000);
}

const editando = computed(() => {
    return props.id
})


</script>

<template>

    <div class="md:w-1/3">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>
        
        <Alerta 
            v-if="alerta.mensaje"
            :alerta="alerta"
        />

        <form 
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" a
            @submit.prevent="validar"
        >
            <div class="mb-5">

                <label 
                    for="mascota" 
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Mascota
                </label>
                <input 
                    id="mascota" 
                    type="text" 
                    placeholder="Nombre del mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-xl"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value)"
                >
            </div>
            <div class="mb-5">
                <label 
                    for="propietario" 
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Propietario
                </label>
                <input 
                    id="propietario" 
                    type="text" 
                    placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-xl"
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value)"
                >
            </div>
            <div class="mb-5">
                <label 
                    for="email" 
                    class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                <input 
                    id="email" 
                    type="email" 
                    placeholder="Email"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-xl"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                >
            </div>
            <div class="mb-5">
                <label 
                    for="alta" 
                    class="block text-gray-700 uppercase font-bold"
                >
                    Fecha
                </label>
                <input 
                    id="alta" 
                    type="date" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-xl"
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value)"
                >
            </div>
            <div class="mb-5">
                <label
                    for="sintomas" 
                    class="block text-gray-700 uppercase font-bold"
                >
                    Sintomas
                </label>
                <textarea 
                    id="sintomas" 
                    placeholder="Describe los sintomas" 
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-xl h-40"
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value)"
                >
                </textarea>
            </div>
            <input 
                type="submit"
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg
                indigo-700 cursor-pointer transition-colors rounded-xl"
                :value="[editando ? 'Actualizar Registro' : 'Guardar Registro']"
            
            >
        </form>
    </div>

</template>
