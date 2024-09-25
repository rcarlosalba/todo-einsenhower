<script setup>
import { computed } from 'vue'

const props = defineProps({
  isOpen: Boolean,
  taskName: String
})

const emit = defineEmits(['close'])

const motivationalMessages = [
  "¡Excelente trabajo! Sigue así.",
  "Un paso más hacia tus objetivos. ¡Bien hecho!",
  "Tu dedicación está dando frutos. ¡Continúa así!",
  "Cada tarea completada es un logro. ¡Enhorabuena!",
  "¡Estás en racha! Mantén ese impulso."
]

const motivationalMessage = computed(() => {
  const randomIndex = Math.floor(Math.random() * motivationalMessages.length)
  return motivationalMessages[randomIndex]
})

const close = () => {
  emit('close')
}
</script>
<template>
    <div v-if="isOpen" class="modal-overlay">
      <div class="modal-content congratulations-modal">
        <h2>¡Felicidades!</h2>
        <p>Has completado la tarea: <strong>{{ taskName }}</strong></p>
        <p class="motivational-message">{{ motivationalMessage }}</p>
        <div class="firework"></div>
        <div class="firework"></div>
        <div class="firework"></div>
        <button @click="close" class="btn btn-primary mt-3">Cerrar</button>
      </div>
    </div>
  </template>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  
  .modal-content {
    background-color: white;
    padding: 2rem;
    border-radius: 8px;
    text-align: center;
    position: relative;
    overflow: hidden;
  }
  
  .congratulations-modal {
    background: linear-gradient(45deg, #6ab7ff, #ff7eb3);
    color: white;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  }
  
  .motivational-message {
    font-style: italic;
    margin-top: 1rem;
  }
  
  .firework {
    position: absolute;
    width: 5px;
    height: 5px;
    border-radius: 50%;
    box-shadow: 0 0 0 4px rgba(255,255,255,0.1), 0 0 0 8px rgba(255,255,255,0.1), 0 0 20px rgba(255,255,255,1);
    animation: firework 1s ease-out infinite;
  }
  
  .firework:nth-child(2) {
    animation-delay: 0.25s;
    left: 30%;
  }
  
  .firework:nth-child(3) {
    animation-delay: 0.5s;
    left: 70%;
  }
  
  @keyframes firework {
    0% { transform: translateY(0) scale(1); opacity: 1; }
    100% { transform: translateY(-100px) scale(0.1); opacity: 0; }
  }
  </style>