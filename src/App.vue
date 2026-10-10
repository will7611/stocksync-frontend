<template>
  <div class="min-h-screen bg-gray-50 font-sans text-gray-800">
    <nav class="bg-white shadow p-4 flex justify-between items-center">
      <div class="text-2xl font-bold text-blue-600">StockSync</div>
      <a href="#contacto" class="bg-blue-600 text-white px-4 py-2 rounded hover:bg-blue-700 transition">Solicitar Demo</a>
    </nav>

    <header class="flex flex-col md:flex-row items-center justify-between p-10 md:p-20 bg-blue-50">
      <div class="md:w-1/2 mb-10 md:mb-0">
        <h1 class="text-4xl md:text-5xl font-extrabold mb-4 leading-tight">
          Gestión de Inventario Inteligente para tu Empresa
        </h1>
        <p class="text-lg text-gray-600 mb-6">
          Controla entradas, salidas y disponibilidad en tiempo real. Evita el desabastecimiento con alertas automáticas.
        </p>
        <a href="#contacto" class="px-6 py-3 bg-blue-600 text-white font-semibold rounded shadow-lg hover:bg-blue-700 transition">
          Empieza Gratis
        </a>
      </div>
      <div class="md:w-1/2 flex justify-center">
        <div class="w-full max-w-md h-64 bg-gray-200 rounded-xl shadow-inner flex items-center justify-center text-gray-500 text-xl font-medium">
          Dashboard interactivo
        </div>
      </div>
    </header>

    <section id="contacto" class="py-20 px-10 flex justify-center">
      <div class="bg-white p-8 rounded-xl shadow-xl w-full max-w-lg">
        <h2 class="text-2xl font-bold mb-2 text-center">Solicita una Demostración</h2>
        <p class="text-center text-gray-500 mb-6">Déjanos tus datos y nos pondremos en contacto.</p>
        
        <form @submit.prevent="enviarFormulario" class="space-y-4">
          <div>
            <label class="block text-sm font-medium text-gray-700">Nombre Completo</label>
            <input v-model="formulario.nombre" type="text" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 border focus:border-blue-500 focus:ring-blue-500">
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700">Empresa</label>
            <input v-model="formulario.empresa" type="text" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 border focus:border-blue-500 focus:ring-blue-500">
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700">Correo Electrónico</label>
            <input v-model="formulario.email" type="email" required class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-2 border focus:border-blue-500 focus:ring-blue-500">
          </div>
          
          <button type="submit" :disabled="cargando" class="w-full bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 transition disabled:opacity-50">
            {{ cargando ? 'Procesando...' : 'Enviar Solicitud' }}
          </button>
        </form>

        <div v-if="mensajeRespuesta" :class="['mt-4 p-3 rounded text-sm', error ? 'bg-red-100 text-red-700' : 'bg-green-100 text-green-700']">
          {{ mensajeRespuesta }}
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const formulario = ref({
  nombre: '',
  empresa: '',
  email: ''
});

const cargando = ref(false);
const mensajeRespuesta = ref('');
const error = ref(false);

const enviarFormulario = async () => {
  cargando.value = true;
  mensajeRespuesta.value = '';
  error.value = false;

  try {
    const respuesta = await axios.post('http://localhost:8000/api/v1/solicitar-demo', formulario.value);
    
    mensajeRespuesta.value = respuesta.data.message;
    formulario.value = { nombre: '', empresa: '', email: '' };
  } catch (err) {
    error.value = true;
    mensajeRespuesta.value = err.response?.data?.message || 'Error de conexión. Asegúrate de que el Backend esté corriendo.';
  } finally {
    cargando.value = false;
  }
};
</script>