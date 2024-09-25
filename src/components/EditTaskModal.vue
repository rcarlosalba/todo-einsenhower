<template>
    <div v-if="isOpen" class="modal-overlay">
      <div class="modal-content">
        <h3>Editar Tarea</h3>
        <form @submit.prevent="submitForm">
          <div class="mb-3">
            <label for="editTaskName" class="form-label">Nombre de la tarea</label>
            <input v-model="editedTask.name" type="text" class="form-control" id="editTaskName" required>
          </div>
          <div class="mb-3">
            <label for="editTaskDescription" class="form-label">Descripción de la tarea</label>
            <textarea v-model="editedTask.description" class="form-control" id="editTaskDescription" rows="3"></textarea>
          </div>
          <div class="mb-3">
            <div class="form-check">
              <input v-model="editedTask.important" class="form-check-input" type="checkbox" id="editImportantCheck">
              <label class="form-check-label" for="editImportantCheck">Importante</label>
            </div>
            <div class="form-check">
              <input v-model="editedTask.urgent" class="form-check-input" type="checkbox" id="editUrgentCheck">
              <label class="form-check-label" for="editUrgentCheck">Urgente</label>
            </div>
          </div>
          <div class="d-flex justify-content-end">
            <button type="button" class="btn btn-secondary me-2" @click="closeModal">Cancelar</button>
            <button type="submit" class="btn btn-primary">Guardar Cambios</button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, watch } from 'vue'
  
  const props = defineProps({
    isOpen: Boolean,
    task: Object
  })
  
  const emit = defineEmits(['update-task', 'close'])
  
  const editedTask = ref({ ...props.task })
  
  watch(() => props.task, (newTask) => {
    editedTask.value = { ...newTask }
  })
  
  const submitForm = () => {
    emit('update-task', editedTask.value)
    closeModal()
  }
  
  const closeModal = () => {
    emit('close')
  }
  </script>
  
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
    width: 90%;
    max-width: 500px;
  }
  </style>