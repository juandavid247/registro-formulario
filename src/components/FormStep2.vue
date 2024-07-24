<template>
  <div class="max-w-xl mx-auto p-8 rounded shadow-lg" @mousemove="handleMouseMove" @mouseleave="resetTransform">
    <div class="light" ref="light"></div>
    <h2 class="text-2xl mb-4 text-center text-gradient">Paso 2: Información de Contacto</h2>
    <form @submit.prevent="next"
      class="grid grid-cols-2 sm:grid-cols-2 md:grid-cols-2 lg:grid-cols-2 xl:grid-cols-2 gap-2">
      <!-- Correo electrónico -->
      <div class="mb-4 relative">
        <label for="email" class="block text-sm font-medium text-gray-700">Correo electrónico</label>
        <div class="relative">
          <i
            class="fas fa-envelope absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input type="email" id="email" v-model="localForm.email" @input="validateEmail" @focus="animateIcon"
            @blur="resetIcon" :class="{ 'border-red-500': !validations.email, 'border-gray-300': validations.email }"
            class="mt-1 block w-full pl-10 form-input text-gray-700" required />
          <i v-if="localForm.email !== '' && validations.email"
            class="fas fa-check-circle absolute right-0 top-1/2 transform -translate-y-1/2 text-green-500 icon-adjust"></i>
        </div>
        <p v-if="!validations.email" class="text-red-500 text-sm">Correo electrónico no válido.</p>
      </div>

      <!-- Contraseña -->
      <div class="mb-4 relative">
        <label for="password" class="block text-sm font-medium text-gray-700">Contraseña</label>
        <div class="relative">
          <i class="fas fa-lock absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input type="password" id="password" v-model="localForm.password" @input="validatePasswordMatch"
            @focus="animateIcon" @blur="resetIcon"
            :class="{ 'border-red-500': !validations.passwordsMatch, 'border-gray-300': validations.passwordsMatch }"
            class="mt-1 block w-full pl-10 form-input font-medium text-gray-700" required />
          <i v-if="localForm.password !== '' && validations.passwordsMatch"
            class="fas fa-check-circle absolute right-0 top-1/2 transform -translate-y-1/2 text-green-500 icon-adjust"></i>
        </div>
      </div>
      <!-- Confirmar Contraseña -->
      <div class="mb-4 relative">
        <label for="confirmPassword" class="block text-sm font-medium text-gray-700">Confirmar Contraseña</label>
        <div class="relative">
          <i class="fas fa-lock absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input type="password" id="confirmPassword" v-model="localForm.confirmPassword" @input="validatePasswordMatch"
            @focus="animateIcon" @blur="resetIcon"
            :class="{ 'border-red-500': !validations.passwordsMatch, 'border-gray-300': validations.passwordsMatch }"
            class="mt-1 block w-full pl-10 form-input font-medium text-gray-700" required />
          <i v-if="localForm.confirmPassword !== '' && validations.passwordsMatch"
            class="fas fa-check-circle absolute right-0 top-1/2 transform -translate-y-1/2 text-green-500 icon-adjust"></i>
        </div>
        <p v-if="!validations.passwordsMatch" class="text-red-500 text-sm">Las contraseñas no coinciden.</p>
      </div>
      <!-- Número de teléfono -->
      <div class="mb-4 relative">
        <label for="telefono" class="block text-sm font-medium text-gray-700">Número de teléfono</label>
        <div class="relative">
          <i class="fas fa-phone absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input type="tel" id="telefono" v-model="localForm.telefono" @input="validateTelefono" @focus="animateIcon"
            @blur="resetIcon"
            :class="{ 'border-red-500': !validations.telefono, 'border-gray-300': validations.telefono }"
            class="mt-1 block w-full pl-10 form-input text-gray-700" required />
          <i v-if="localForm.telefono !== '' && validations.telefono"
            class="fas fa-check-circle absolute right-0 top-1/2 transform -translate-y-1/2 text-green-500 icon-adjust"></i>
        </div>
        <p v-if="!validations.telefono" class="text-red-500 text-sm">Número de teléfono no válido.</p>
      </div>
      <!-- Número de celular -->
      <div class="mb-4 relative">
        <label for="celular" class="block text-sm font-medium text-gray-700">Número de celular</label>
        <div class="relative">
          <i
            class="fas fa-mobile-alt absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input type="tel" id="celular" v-model="localForm.celular" @input="validateCelular" @focus="animateIcon"
            @blur="resetIcon"
            :class="{ 'border-red-500': !validations.celular, 'border-gray-300': validations.celular }"
            class="mt-1 block w-full pl-10 form-input text-gray-700" required />
          <i v-if="localForm.celular !== '' && validations.celular"
            class="fas fa-check-circle absolute right-0 top-1/2 transform -translate-y-1/2 text-green-500 icon-adjust"></i>
        </div>
        <p v-if="!validations.celular" class="text-red-500 text-sm">Número de celular no válido.</p>
      </div>
      <!-- Botones de navegación -->
      <div class="col-span-2 flex justify-between">
        <button type="button" class="custom-prev-btn" @click="previous">Anterior</button>
        <button type="submit" class="custom-submit-btn"
          :disabled="!validations.email || !validations.passwordsMatch || !validations.telefono || !validations.celular"
          :class="{ 'opacity-50': !validations.email || !validations.passwordsMatch || !validations.telefono || !validations.celular }">Siguiente</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'FormStep2',
  props: {
    form: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      localForm: { ...this.form },
      validations: {
        email: true,
        passwordsMatch: true,
        telefono: true,
        celular: true
      }
    };
  },
  methods: {
    validateEmail() {
      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      this.validations.email = emailPattern.test(this.localForm.email);
    },
    validatePasswordMatch() {
      this.validations.passwordsMatch = this.localForm.password === this.localForm.confirmPassword;
    },
    validateTelefono() {
      const phonePattern = /^[0-9]{7,10}$/;
      this.validations.telefono = phonePattern.test(this.localForm.telefono);
    },
    validateCelular() {
      const phonePattern = /^[0-9]{10}$/;
      this.validations.celular = phonePattern.test(this.localForm.celular);
    },
    next() {
      this.validateEmail();
      this.validatePasswordMatch();
      this.validateTelefono();
      this.validateCelular();

      if (this.validations.email && this.validations.passwordsMatch && this.validations.telefono && this.validations.celular) {
        this.$emit('update:form', this.localForm);
        this.$emit('next');
      }
    },
    previous() {
      this.$emit('previous');
    },
    animateIcon(event) {
      console.log('Focus event triggered');
      const icon = event.target.previousElementSibling;
      if (icon && icon.classList.contains('icon-animate')) {
        icon.classList.add('focused');
      }
    },
    resetIcon(event) {
      console.log('Blur event triggered');
      const icon = event.target.previousElementSibling;
      if (icon && icon.classList.contains('icon-animate')) {
        icon.classList.remove('focused');
      }
    },

    handleMouseMove(event) {
      const light = this.$refs.light;
      if (!light) return; // Verificar que el elemento exista
      const card = this.$el; // Acceder al elemento raíz del componente
      const rect = card.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      light.style.left = `${x}px`;
      light.style.top = `${y}px`;
      light.style.opacity = '1';
    },
    resetTransform() {
      const light = this.$refs.light;
      if (!light) return; // Verificar que el elemento exista

      light.style.opacity = '0';
      light.style.left = '-1000px';
      light.style.top = '-1000px';

      const card = this.$el; // Acceder al elemento raíz del componente
      card.style.transform = '';
    }
  },
  mounted() {
    // Acciones que deben realizarse después de que el componente esté completamente montado
    this.$nextTick(() => {
      // Ejemplo: inicialización de referencia light
      const light = this.$refs.light;
      if (light) {
        // Lógica de inicialización aquí si es necesario
      }
    });
  }
};
</script>
<style scoped>
.max-w-xl {
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

.form-input {
  font-size: 0.875rem;
  line-height: 1.25rem;
  border-radius: 0.25rem;
  border: 1px solid #e2e8f0;
  background-color: #f7fafc;
  height: 2.5rem;
}

.text-gray-700 {
  color: #718096;
}

.custom-submit-btn,
.custom-prev-btn {
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

.opacity-50 {
  opacity: 0.5;
  cursor: not-allowed;
}

.icon-adjust {
  font-size: 1rem;
  /* Ajusta el tamaño según tus necesidades */
  position: absolute;
  right: 0.5rem;
  transform: translateY(-50%);
  transition: opacity 0.3s ease;
  opacity: 1;
  /* Oculto por defecto */
}

.form-input:not(:placeholder-shown)+.icon-adjust {
  opacity: 1;
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

@media (max-width: 480px) {
  .icon-adjust {
    font-size: 1rem;
    /* Ajusta el tamaño según tus necesidades */
    position: absolute;
    right: 0.2rem;
    top: 25%;
    transform: translateY(-50%);
    transition: opacity 0.3s ease;
    opacity: 1;
    /* Oculto por defecto */
  }
}
</style>
