<script setup>
defineProps({
  isOpen: Boolean,
  task: Object
})

defineEmits(['close', 'edit', 'complete', 'delete'])
</script>
<template>
    <div v-if="isOpen" class="modal-overlay">
      <div class="modal-content">
        <h3>{{ task.name }}</h3>
        <p>{{ task.description }}</p>
        <div class="task-details">
          <span :class="['badge', { 'bg-success': task.important, 'bg-secondary': !task.important }]">
            {{ task.important ? 'Importante' : 'No importante' }}
          </span>
          <span :class="['badge', { 'bg-danger': task.urgent, 'bg-warning': !task.urgent }]">
            {{ task.urgent ? 'Urgente' : 'No urgente' }}
          </span>
        </div>
        <div class="action-buttons">
          <button @click="$emit('edit')" class="btn btn-primary">
            <i class="bi bi-pencil"></i> Editar
          </button>
          <button @click="$emit('complete')" class="btn btn-success" :disabled="task.completed">
            <i class="bi bi-check-lg"></i> Completar
          </button>
          <button @click="$emit('delete')" class="btn btn-danger">
            <i class="bi bi-trash"></i> Eliminar
          </button>
        </div>
        <button @click="$emit('close')" class="btn btn-secondary mt-3">Cerrar</button>
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
    width: 90%;
    max-width: 500px;
  }
  
  .task-details {
    margin-top: 1rem;
    margin-bottom: 1rem;
  }
  
  .badge {
    margin-right: 0.5rem;
  }
  
  .action-buttons {
    display: flex;
    justify-content: space-between;
    margin-top: 1rem;
  }
  </style>