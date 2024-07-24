<template>
  <div class="container mx-auto mt-10 bg-customBackground">
    <ProgressBar :currentStep="currentStep" />
    <div v-if="currentStep === 1">
      <Step1 @next="nextStep" :form="form" @updateForm="updateForm" />
    </div>
    <div v-if="currentStep === 2">
      <Step2 @next="nextStep" @previous="previousStep" :form="form" @updateForm="updateForm" />
    </div>
    <div v-if="currentStep === 3">
      <Step3 @previous="previousStep" @submit="submitForm" :form="form" @updateForm="updateForm" />
    </div>
  </div>
</template>

<script>
import ProgressBar from './components/ProgressBar.vue'
import Step1 from './components/FormStep1.vue'
import Step2 from './components/FormStep2.vue'
import Step3 from './components/FormStep3.vue'

export default {
  components: { ProgressBar, Step1, Step2, Step3 },
  data() {
    return {
      currentStep: 1,
      form: {
        pais: '',
        genero: '',
        primerNombre: '',
        segundoNombre: '',
        fechaNacimiento: '',
        tipoDocumento: '',
        numeroDocumento: '',
        fotoFrente: null,
        fotoReverso: null,
        email: '',
        password: '',
        confirmPassword: '',
        telefono: '',
        celular: '',
        direccion: '',
        codigoPostal: ''
      }
    }
  },
  methods: {
    nextStep() {
      this.currentStep++
    },
    previousStep() {
      this.currentStep--
    },
    updateForm(field, value) {
      this.form[field] = value
    },
    submitForm() {
      console.log(this.form)
      alert('Formulario enviado con éxito')
    }
  }
}
</script>

<style>
@import 'tailwindcss/tailwind.css';

body {
  @apply bg-customBackground;
}
</style>
