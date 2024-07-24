<template>
  <div class="flex items-center justify-center mb-8">
    <div v-for="step in 3" :key="step" class="flex items-center">
      <div 
        class="step-circle"
        :class="[
          { 
            'step-completed': currentStep > step, 
            'step-current': currentStep === step,
            'step-pending': currentStep < step 
          }
        ]"
        @mouseover="handleStepHover(step)"
        @mouseleave="resetStepHover()"
      >
        <span 
          class="step-text"
          :class="[
            { 
              'step-text-completed': currentStep > step, 
              'step-text-current': currentStep === step,
              'step-text-pending': currentStep < step 
            }
          ]"
        >
          {{ step }}
        </span>
        <span class="neon"></span>
      </div>
      <div v-if="step < 3" :class="['step-line', { 'step-line-completed': currentStep > step }]"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['currentStep'],
  methods: {
    handleStepHover(step) {
      if (this.currentStep < step) {
        this.$refs[`step${step}`][0].classList.add('step-hover');
      }
    },
    resetStepHover() {
      document.querySelectorAll('.step-circle').forEach((el) => {
        el.classList.remove('step-hover');
      });
    },
  },
};
</script>

<style scoped>
.flex {
  display: flex;
}
.items-center {
  align-items: center;
}
.justify-center {
  justify-content: center;
}
.mb-8 {
  margin-bottom: 2rem;
}

.step-circle {
  position: relative;
  border-radius: 50%; 
  height: 3rem; 
  width: 3rem; 
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s ease, transform 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  overflow: hidden;
}

.step-completed {
  background-color: #6cb2eb;
  border-radius: 50px; /* Ajusta el radio según tu diseño */
  box-shadow: 0 0 10px #6cb2eb, 0 0 20px #6cb2eb, 0 0 40px #6cb2eb, 0 0 80px #6cb2eb; /* Efecto de resplandor */
}

@keyframes neon-glow {
  from {
    filter: brightness(1.2) blur(15px); /* Ajusta el brillo y el desenfoque según sea necesario */
  }
  to {
    filter: brightness(1.5) blur(20px); /* Ajusta el brillo y el desenfoque según sea necesario */
  }
}


.step-current {
  background-color: #9f7aea;
  border-radius: 50px; /* Ajusta el radio según tu diseño */
  box-shadow: 0 0 15px #9f7aea, 0 0 25px #9f7aea, 0 0 40px #9f7aea, 0 0 80px #9f7aea; /* Efecto de resplandor */
  animation: neon-glow 1.5s infinite alternate; /* Animación del efecto neon */
}

.step-pending {
  background-color: #cfd8dc; 
}

.step-hover::before, .step-hover::after {
  content: "";
  position: absolute;
  background-color: inherit;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  opacity: 0.5;
  pointer-events: none;
}

.step-hover::before {
  animation: neon-glow 1.5s infinite alternate;
}

.step-hover::after {
  animation: neon-glow 1.5s infinite alternate-reverse;
}

@keyframes neon-glow {
  0% {
    transform: scale(0.8);
    opacity: 0.8;
  }
  100% {
    transform: scale(1.2);
    opacity: 1;
  }
}

.step-text {
  transition: color 0.3s ease;
  color: #FFFFFF;
  font-weight: bold;
  font-size: 1.2rem;
}

.step-text-completed {
  color: #FFFFFF; 
}

.step-text-current {
  color: #FFFFFF; 
}

.step-text-pending {
  color: #546e7a; 
}

.step-line {
  height: 0.4rem; 
  width: 20rem; 
  background-color: #cfd8dc; 
  transition: background-color 0.3s ease;
}

.step-line-completed {
  background: linear-gradient(90deg, #6cb2eb, #9f7aea); /* Gradiente de azul a morado */
  border-radius: 50px; /* Ajusta el radio según tu diseño */
  box-shadow: 0 0 10px #6cb2eb, 0 0 20px #6cb2eb, 0 0 40px #9f7aea, 0 0 80px #9f7aea; /* Efecto de resplandor */
  animation: neon-glow; /* Animación del efecto neon */
}

/* Media queries */
@media (max-width: 1024px) { 
  .step-line {
    width: 15rem;
  }
}

@media (max-width: 768px) { 
  .step-line {
    width: 10rem;
  }
}

@media (max-width: 640px) { 
  .step-line {
    width: 8rem; 
  }
}

@media (max-width: 480px) { 
  .step-circle {
    height: 2.5rem; 
    width: 2.5rem; 
  }

  .step-line {
    width: 6rem; 
  }

  .step-text {
    font-size: 1rem; 
  }
}
</style>
