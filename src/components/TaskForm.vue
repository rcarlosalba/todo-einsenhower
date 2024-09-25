<script setup>
  import { ref, computed } from 'vue'
  
  // Estado del formulario
  const taskName = ref('')
  const taskDescription = ref('')
  const isImportant = ref(false)
  const isUrgent = ref(false)
  
  // Computed property para determinar el color de fondo
  const backgroundColor = computed(() => {
    if (isImportant.value && isUrgent.value) return 'green'
    if (isImportant.value && !isUrgent.value) return 'orange'
    if (!isImportant.value && isUrgent.value) return 'blue'
    return 'yellow'
  })
  
  // Función para manejar el envío del formulario
  const emit = defineEmits(['add-task'])
  
  const submitForm = () => {
    const newTask = {
      name: taskName.value,
      description: taskDescription.value,
      important: isImportant.value,
      urgent: isUrgent.value,
      color: backgroundColor.value,
      completed: false
    }
    
    emit('add-task', newTask)
    
    // Limpiar el formulario
    taskName.value = ''
    taskDescription.value = ''
    isImportant.value = false
    isUrgent.value = false
  }
  </script>
  
<template>
    <form @submit.prevent="submitForm" class="task-form">
      <div class="mb-3">
        <label for="taskName" class="form-label">Nombre de la tarea</label>
        <input v-model="taskName" type="text" class="form-control" id="taskName" required>
      </div>
      <div class="mb-3">
        <label for="taskDescription" class="form-label">Descripción de la tarea</label>
        <textarea v-model="taskDescription" class="form-control" id="taskDescription" rows="3"></textarea>
      </div>
      <div class="mb-3">
        <div class="form-check">
          <input v-model="isImportant" class="form-check-input" type="checkbox" id="importantCheck">
          <label class="form-check-label" for="importantCheck">Importante</label>
        </div>
        <div class="form-check">
          <input v-model="isUrgent" class="form-check-input" type="checkbox" id="urgentCheck">
          <label class="form-check-label" for="urgentCheck">Urgente</label>
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Guardar Tarea</button>
    </form>
  </template>
  
  <style scoped>
  .task-form {
    max-width: 500px;
    margin: 0 auto;
  }
  </style>