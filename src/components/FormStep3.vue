<template>
  <div class="max-w-xl mx-auto p-8 rounded shadow-lg relative overflow-hidden" @mousemove="handleMouseMove" @mouseleave="resetTransform">
    <div class="light" ref="light"></div>
    <h2 class="text-2xl mb-4 text-center text-gradient">Paso 3: Dirección de Residencia</h2>
    <form @submit.prevent="submit" class="grid grid-cols-2 sm:grid-cols-2 md:grid-cols-2 lg:grid-cols-2 xl:grid-cols-2 gap-2">
      <!-- Dirección de residencia -->
      <div class="mb-4 relative">
        <label for="direccion" class="block text-sm font-medium text-gray-700">Dirección</label>
        <div class="relative">
          <i class="fas fa-home absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input type="text" id="direccion" v-model="localForm.direccion"
            :class="{ 'border-red-500': !validations.direccion, 'border-gray-300': validations.direccion }"
            class="mt-1 block w-full pl-10 form-input text-gray-600" required @focus="animateIcon" @blur="resetIcon" />
          <i v-if="localForm.direccion !== ''" class="fas fa-check-circle absolute right-0 top-1/2 transform -translate-y-1/2 text-green-500 icon-adjust"></i>
        </div>
      </div>

      <!-- Código postal -->
      <div class="mb-4 relative">
        <label for="codigoPostal" class="block text-sm font-medium text-gray-700">Código postal</label>
        <div class="relative">
          <i class="fas fa-map-pin absolute left-3 top-1/2 transform -translate-y-1/2 custom-icon-color icon-animate"></i>
          <input type="text" id="codigoPostal" v-model="localForm.codigoPostal"
            :class="{ 'border-red-500': !validations.codigoPostal, 'border-gray-300': validations.codigoPostal }"
            class="mt-1 block w-full pl-10 form-input text-gray-600" required @focus="animateIcon" @blur="resetIcon" />
          <i v-if="localForm.codigoPostal !== ''" class="fas fa-check-circle absolute right-0 top-1/2 transform -translate-y-1/2 text-green-500 icon-adjust"></i>
        </div>
      </div>

      <!-- Botones de navegación -->
      <div class="col-span-2 flex justify-between">
        <button type="button" class="custom-prev-btn" @click="previous">Anterior</button>
        <button type="submit" class="custom-submit-btn" :disabled="!validarCampos()" :class="{ 'opacity-50': !validarCampos() }">Enviar</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'FormStep3',
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
        direccion: true,
        codigoPostal: true
      }
    };
  },
  methods: {
    validarCampos() {
      this.validations.direccion = this.localForm.direccion !== '';
      this.validations.codigoPostal = this.localForm.codigoPostal !== '';
      return this.validations.direccion && this.validations.codigoPostal;
    },
    submit() {
      if (this.validarCampos()) {
        this.$emit('update:form', this.localForm);
        this.$emit('submit');
      }
    },
    previous() {
      this.$emit('previous');
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

      light.style.left = `${x}px`;
      light.style.top = `${y}px`;

      const centerX = rect.width / 2;
      const centerY = rect.height / 2;
      const distance = Math.sqrt(Math.pow(x - centerX, 2) + Math.pow(y - centerY, 2));
      const maxDistance = Math.sqrt(Math.pow(centerX, 2) + Math.pow(centerY, 2));
      const opacity = 1 - distance / maxDistance;

      light.style.opacity = opacity;

      const deltaX = (x - centerX) / centerX;
      const deltaY = (y - centerY) / centerY;
      const rotateX = deltaY * 2;
      const rotateY = deltaX * 2;

      card.style.transform = `perspective(700px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
    },
    resetTransform(event) {
      const card = event.currentTarget;
      card.style.transform = 'perspective(700px) rotateX(0) rotateY(0)';

      const light = this.$refs.light;
      light.style.opacity = 0;
    }
  },
  watch: {
    'localForm.direccion'(newValue) {
      const checkIcon = document.querySelector('.icon-adjust');
      if (checkIcon) {
        if (newValue !== '') {
          checkIcon.classList.add('appear');
        } else {
          checkIcon.classList.remove('appear');
        }
      }
    },
    'localForm.codigoPostal'(newValue) {
      const checkIcon = document.querySelector('.icon-adjust');
      if (checkIcon) {
        if (newValue !== '') {
          checkIcon.classList.add('appear');
        } else {
          checkIcon.classList.remove('appear');
        }
      }
    }
  }
};
</script>

<style scoped>
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
  padding: 0.5rem 1rem;
  font-size: 0.875rem;
  line-height: 1.25rem;
  border-radius: 0.25rem;
  border: 1px solid #e2e8f0;
  background-color: #f7fafc;
  padding-left: 2.5rem;
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
  position: absolute;
  right: 0.5rem;
  transform: translateY(-50%);
  transition: opacity 0.3s ease;
  opacity: 1;
}

.icon-animate {
  transition: transform 0.3s ease, color 0.3s ease;
}

.icon-animate.focused {
  animation: move-icon 0.6s forwards;
  color: #3182ce;
}

.custom-icon-color {
  color: #9f7aea;
}

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
    position: absolute;
    right: 0.2rem;
    top: 25%;
    transform: translateY(-50%);
    transition: opacity 0.3s ease;
    opacity: 1;
  }
}
</style>
