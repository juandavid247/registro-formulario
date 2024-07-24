<template>
  <div class="max-w-3xl mx-auto p-4 md:p-8 rounded shadow-lg" @mousemove="handleMouseMove" @mouseleave="resetTransform">
    <div class="light" ref="light"></div>
    <h2 class="text-2xl mb-4 text-center text-gradient">Paso 1: Información Personal</h2>
    <form @submit.prevent="next" class="grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-2 xl:grid-cols-2 gap-2 card-skew">

      <!-- País -->
      <div class="mb-4 relative">
        <label for="pais" class="block text-sm font-medium text-gray-600">País</label>
        <div class="relative">
          <i class="fas fa-globe absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <select v-model="localForm.pais" id="pais" class="mt-1 block w-full pl-10 pr-10 form-select text-gray-600" required @focus="animateIcon" @blur="resetIcon">
            <option disabled value="">Seleccione un país</option>
            <option v-for="pais in paises" :key="pais.name" :value="pais.name" class="font-medium text-gray-600">{{ pais.name }}</option>
          </select>
          <i v-if="localForm.pais" class="fas fa-check-circle absolute right-3 top-1/2 transform -translate-y-1/2 text-green-500"></i>
        </div>
      </div>

      <!-- Género -->
      <div class="mb-4 relative">
        <label for="genero" class="block text-sm font-medium text-gray-600">Género</label>
        <div class="relative">
          <i class="fas fa-venus-mars absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <select v-model="localForm.genero" id="genero" class="mt-1 pl-10 block w-full form-select text-gray-600" required @focus="animateIcon" @blur="resetIcon">
            <option disabled value="">Seleccione un género</option>
            <option value="Masculino">Masculino</option>
            <option value="Femenino">Femenino</option>
            <option value="Otro">Otro</option>
          </select>
          <i v-if="localForm.genero" class="fas fa-check-circle absolute right-3 top-1/2 transform -translate-y-1/2 text-green-500"></i>
        </div>
      </div>

      <!-- Primer nombre -->
      <div class="mb-4 relative">
        <label for="primerNombre" class="block text-sm font-medium text-gray-600">Primer Nombre</label>
        <div class="relative">
          <i class="fas fa-user absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input v-model="localForm.primerNombre" id="primerNombre" type="text" class="mt-1 pl-10 block w-full form-input text-gray-600" required @focus="animateIcon" @blur="resetIcon">
          <i v-if="localForm.primerNombre" class="fas fa-check-circle absolute right-3 top-1/2 transform -translate-y-1/2 text-green-500"></i>
        </div>
      </div>

      <!-- Segundo nombre -->
      <div class="mb-4 relative">
        <label for="segundoNombre" class="block text-sm font-medium text-gray-600">Segundo Nombre</label>
        <div class="relative">
          <i class="fas fa-user-plus absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input v-model="localForm.segundoNombre" id="segundoNombre" type="text" class="mt-1 pl-10 block w-full form-input text-gray-600" @focus="animateIcon" @blur="resetIcon">
          <i v-if="localForm.segundoNombre" class="fas fa-check-circle absolute right-3 top-1/2 transform -translate-y-1/2 text-green-500"></i>
        </div>
      </div>

      <!-- Fecha de nacimiento -->
      <div class="mb-4 relative">
        <label for="fechaNacimiento" class="block text-sm font-medium text-gray-600">Fecha de Nacimiento</label>
        <div class="relative">
          <i class="fas fa-calendar-alt absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input v-model="localForm.fechaNacimiento" id="fechaNacimiento" type="date" class="mt-1 pl-10 block w-full form-input text-gray-600" required @focus="animateIcon" @blur="resetIcon">
          <i v-if="localForm.fechaNacimiento && edadValida" class="fas fa-check-circle absolute right-3 top-1/2 transform -translate-y-1/2 text-green-500"></i>
          <p v-if="!edadValida" class="absolute mt-1 text-sm text-red-500">Ingrese una edad mayor a 18 años</p>
        </div>
      </div>

      <!-- Tipo documento -->
      <div class="mb-4 relative">
        <label for="tipoDocumento" class="block text-sm font-medium text-gray-600">Tipo de Documento</label>
        <div class="relative">
          <i class="fas fa-id-card absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <select v-model="localForm.tipoDocumento" id="tipoDocumento" class="mt-1 pl-10 block w-full form-select text-gray-600" required @focus="animateIcon" @blur="resetIcon">
            <option disabled value="">Seleccione un tipo de documento</option>
            <option value="Cédula de ciudadanía">Cédula de ciudadanía</option>
            <option value="Pasaporte">Pasaporte</option>
            <option value="Cédula de extranjería">Cédula de extranjería</option>
          </select>
          <i v-if="localForm.tipoDocumento" class="fas fa-check-circle absolute right-3 top-1/2 transform -translate-y-1/2 text-green-500"></i>
        </div>
      </div>

      <!-- Número documento -->
      <div class="mb-4 mt-4 relative">
        <label for="numeroDocumento" class="block text-sm font-medium text-gray-600">Número de Documento</label>
        <div class="relative">
          <i class="fas fa-id-badge absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input v-model="localForm.numeroDocumento" id="numeroDocumento" type="text" pattern="[0-9]*" class="mt-1 pl-10 block w-full form-input text-gray-600" minlength="5" required @focus="animateIcon" @blur="resetIcon">
          <i v-if="localForm.numeroDocumento" class="fas fa-check-circle absolute right-3 top-1/2 transform -translate-y-1/2 text-green-500"></i>
        </div>
      </div>
      
      <!-- Foto documento -->
      <div class="mb-4 flex flex-col sm:flex-row md:flex-row justify-center items-center space-y-6 md:space-y-0 md:space-x-6">
        <!-- Foto documento - Frente -->
        <div class="relative">
          <label for="fotoDocumentoFrente" class="block text-sm font-medium text-gray-600">Documento - Frente</label>
          <div class="mt-1 flex items-center">
            <label for="fotoDocumentoFrente" class="cursor-pointer inline-flex items-center px-4 py-2 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-600 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
              <i class="fas fa-camera custom-icon-color icon-animate"></i>
              <span class="ml-2">Foto frontal</span>
            </label>
            <input id="fotoDocumentoFrente" name="fotoDocumentoFrente" type="file" class="sr-only" accept="image/jpeg" @change="handleFileUpload($event, 'fotoDocumentoFrente')" required>
            <i v-if="localForm.fotoDocumentoFrente" class="fas fa-check-circle icon-adjust text-green-500 ml-2"></i>
          </div>
        </div>

        <!-- Foto documento - Reverso -->
        <div class="relative">
          <label for="fotoDocumentoReverso" class="block text-sm font-medium text-gray-600">Documento - Reverso</label>
          <div class="mt-1 flex items-center">
            <label for="fotoDocumentoReverso" class="cursor-pointer inline-flex items-center px-4 py-2 border border-gray-300 rounded-md shadow-sm text-sm font-medium text-gray-600 bg-white hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
              <i class="fas fa-camera custom-icon-color icon-animate"></i>
              <span class="ml-2">Foto reversa</span>
            </label>
            <input id="fotoDocumentoReverso" name="fotoDocumentoReverso" type="file" class="sr-only" accept="image/jpeg" @change="handleFileUpload($event, 'fotoDocumentoReverso')" required>
            <i v-if="localForm.fotoDocumentoReverso" class="fas fa-check-circle icon-adjust text-green-500 ml-2"></i>
          </div>
        </div>
      </div>

      <!-- Botón de siguiente -->
      <div class="mt-6 col-span-1 sm:col-span-2 flex justify-between flex-row-reverse">
        <button type="submit" class="custom-submit-btn" :disabled="!validarCampos()" :class="{ 'opacity-50': !validarCampos() }">Siguiente</button>
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'FormStep1',
  props: {
    form: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      localForm: { ...this.form },
      paises: [],
      edadValida: true // Bandera para verificar la edad
    };
  },
  mounted() {
    this.cargarPaises();
  },
  computed: {
    edad() {
      if (!this.localForm.fechaNacimiento) return null;
      const today = new Date();
      const birthDate = new Date(this.localForm.fechaNacimiento);
      let age = today.getFullYear() - birthDate.getFullYear();
      const month = today.getMonth() - birthDate.getMonth();
      if (month < 0 || (month === 0 && today.getDate() < birthDate.getDate())) {
        age--;
      }
      return age;
    }
  },
  methods: {
    cargarPaises() {
      axios.get(process.env.VUE_APP_COUNTRIES_JSON_URL)
        .then(response => {
          this.paises = response.data.countries;
        })
        .catch(error => {
          console.error('Error al cargar países:', error);
        });
    },
    next() {
      if (this.validarCampos()) {
        this.$emit('update:form', this.localForm);
        this.$emit('next');
      }
    },
    previous() {
      this.$emit('previous');
    },
    handleFileUpload(event, fieldName) {
      const file = event.target.files[0];
      if (!file) return;
      if (file.type === 'image/jpeg' || file.type === 'image/jpg') {
        const reader = new FileReader();
        reader.onload = () => {
          this.localForm[fieldName] = reader.result;
          console.log(`Foto cargada en ${fieldName}:`, this.localForm[fieldName]); // Agrega esta línea para depurar
        };
        reader.readAsDataURL(file);
      } else {
        alert('El archivo debe ser de tipo JPEG.');
        event.target.value = '';
      }
    },
    validarCampos() {
      // Verificar que todos los campos obligatorios estén llenos
      const camposLlenos =
        this.localForm.pais &&
        this.localForm.genero &&
        this.localForm.primerNombre &&
        this.localForm.fechaNacimiento &&
        this.localForm.tipoDocumento &&
        this.localForm.numeroDocumento &&
        this.localForm.fotoDocumentoFrente &&
        this.localForm.fotoDocumentoReverso;

      // Verificar edad mayor de 18 años
      this.edadValida = this.edad >= 18;

      return camposLlenos && this.edadValida;
    },
    animateIcon(event) {
      const icon = event.target.previousElementSibling;
      if (icon && icon.classList.contains('icon-animate')) {
        icon.classList.add('focused');
      }
    },
    resetIcon(event) {
      const icon = event.target.previousElementSibling;
      if (icon && icon.classList.contains('icon-animate')) {
        icon.classList.remove('focused');
      }
    },
    handleMouseMove(event) {
      const light = this.$refs.light;
      const card = event.currentTarget;
      const rect = card.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;
      const centerX = rect.width / 2;
      const centerY = rect.height / 2;
      const deltaX = (x - centerX) / centerX;
      const deltaY = (y - centerY) / centerY;


      light.style.left = `${x}px`;
      light.style.top = `${y}px`;

      const rotateX = deltaY * 2;
      const rotateY = deltaX * 2;

      const distance = Math.sqrt(Math.pow(x - centerX, 2) + Math.pow(y - centerY, 2));
      const maxDistance = Math.sqrt(Math.pow(centerX, 2) + Math.pow(centerY, 2));
      const opacity = 1 - distance / maxDistance;

      light.style.opacity = opacity;

      card.style.transform = `perspective(700px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
    },


    resetTransform(event) {
      const card = event.currentTarget;
      card.style.transform = 'perspective(700px) rotateX(0) rotateY(0)';

      // Ocultar la luz
      const light = this.$refs.light;
      light.style.opacity = 0;
    }
  }
};
</script>


<style scoped>
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}



.max-w-3xl {
  background-color: #161b22;
  color: white;
  border: 1px solid white;
  border-radius: 1rem;
  padding: 2rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), 0 1px 3px rgba(0, 0, 0, 0.08);
  transition: transform 0.3s ease;
  position: relative;
  overflow: hidden;
}

.text-gradient {
  background-image: linear-gradient(to right, #3182ce, #9f7aea, #f43f5e);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

/* Estilos específicos del formulario */
.form-select,
.form-input {
  appearance: none;
  font-size: 1rem;
  line-height: 1.5rem;
  border-radius: 0.5rem;
  border: 1px solid #e2e8f0;
  background-color: #f7fafc;
  width: 100%;
  height: 2.5rem;
}

.bg-blue-500 {
  background-color: #1a73e8;
}

.bg-blue-500:hover {
  background-color: #1e88e5;
}

.bg-blue-500:focus {
  box-shadow: 0 0 0 2px rgba(66, 153, 225, 0.5);
}

.text-red-500 {
  color: #e53e3e;
}

/* Estilos específicos para el efecto de tarjeta */
.max-w-3xl:hover {
  transform: translateY(-5px);
}

.mb-4 {
  margin-bottom: 1rem;
}

.mb-2 {
  margin-bottom: 0.5rem;
}

.custom-submit-btn {
  padding: 0.75rem 2rem;
  font-size: 1rem;
  line-height: 1.5rem;
  border-radius: 2.25rem;
  color: white;
  border: none;
  cursor: pointer;
  transition: background-image 0.3s ease;
}

.custom-submit-btn {
  background-image: linear-gradient(to right, #3182ce, #9f7aea, #f43f5e);
}
.custom-icon-color {
  color: #9f7aea;
}
@keyframes move-icon {
  0% {
    transform: translateY(-50%) scale(1) rotate(0deg);
  }
  25% {
    transform: translateY(-60%) scale(1.1) rotate(10deg);
  }
  50% {
    transform: translateY(-50%) scale(1.2) rotate(-10deg);
  }
  75% {
    transform: translateY(-40%) scale(1.1) rotate(10deg);
  }
  100% {
    transform: translateY(-50%) scale(1) rotate(0deg);
  }
}


.icon-animate {
  transition: transform 0.3s ease, color 0.3s ease;
}

.icon-animate.focused {
  animation: move-icon 0.6s forwards;
  color: #3182ce;
}
.custom-prev-btn {
  background-image: linear-gradient(to right, #ffffff, #b4c0d2, #3182ce);
  color: #3182ce;
}
/* Mostrar el ícono cuando el campo está completado */
.form-input:focus+.fa-check-circle,
.form-select:focus+.fa-check-circle {
  opacity: 1;
}

/* Estilo para el icono */
.icon-adjust {
  font-size: 1rem;
  /* Ajusta el tamaño según tus necesidades */
  position: absolute;
  right: 0.1rem;
  /* Ajusta la distancia desde el borde derecho */
  top: 53%;
  transform: translateY(-50%);
  transition: opacity 0.3s ease;
  opacity: 1;
}

.form-input:not(:placeholder-shown)+.icon-adjust,
.form-select:not(:placeholder-shown)+.icon-adjust,
.form-file:not(:placeholder-shown)+.icon-adjust {
  opacity: 1;
}

.text-red-500 {
  color: #e53e3e;
  font-size: 0.75rem;
}

/* Estilo para la luz */
.light {
  position: absolute;
  width: 230%;
  height: 230%;
  background: radial-gradient(circle, rgba(0, 225, 255, 0.162) 0%, rgba(0, 162, 255, 0.126) 30%, rgba(0, 195, 255, 0.063) 60%, rgba(0, 179, 255, 0) 90%);
  border-radius: 50%;
  pointer-events: none;
  transform: translate(-50%, -50%);
  opacity: 0;
  transition: opacity 0.1s ease;
}
</style>
