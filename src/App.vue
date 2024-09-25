<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import TaskForm from './components/TaskForm.vue'
import TaskQuadrant from './components/TaskQuadrant.vue'
import EditTaskModal from './components/EditTaskModal.vue'
import CongratulationsModal from './components/CongratulationsModal.vue'
import TaskDetailModal from './components/TaskDetailModal.vue'

const tasks = ref([])
const isEditModalOpen = ref(false)
const taskToEdit = ref(null)
const isCongratulationsModalOpen = ref(false)
const completedTaskName = ref('')
const isTaskDetailModalOpen = ref(false)
const taskToView = ref(null)
const currentQuadrantId = ref(null)

const quadrants = computed(() => [
  { 
    id: 'urgentImportant', 
    title: 'Hacer', 
    color: '#8BC34A', 
    tasks: tasks.value.filter(task => task.urgent && task.important)
  },
  { 
    id: 'notUrgentImportant', 
    title: 'Programar', 
    color: '#FFB74D', 
    tasks: tasks.value.filter(task => !task.urgent && task.important)
  },
  { 
    id: 'urgentNotImportant', 
    title: 'Delegar', 
    color: '#64B5F6', 
    tasks: tasks.value.filter(task => task.urgent && !task.important)
  },
  { 
    id: 'notUrgentNotImportant', 
    title: 'Eliminar', 
    color: '#FFD54F', 
    tasks: tasks.value.filter(task => !task.urgent && !task.important)
  }
])

const addTask = (newTask) => {
  tasks.value.push(newTask)
}

const editTask = (quadrantId, index) => {
  const quadrant = quadrants.value.find(q => q.id === quadrantId)
  if (quadrant) {
    taskToEdit.value = { ...quadrant.tasks[index] }
    isEditModalOpen.value = true
  }
}

const updateTask = (updatedTask) => {
  const index = tasks.value.findIndex(task => task.name === taskToEdit.value.name)
  if (index !== -1) {
    tasks.value[index] = updatedTask
  }
}

const closeEditModal = () => {
  isEditModalOpen.value = false
  taskToEdit.value = null
}

const deleteTask = (quadrantId, index) => {
  const quadrant = quadrants.value.find(q => q.id === quadrantId)
  if (quadrant) {
    const taskIndex = tasks.value.findIndex(task => task === quadrant.tasks[index])
    if (taskIndex !== -1) {
      tasks.value.splice(taskIndex, 1)
    }
  }
}

const completeTask = (quadrantId, index) => {
  const quadrant = quadrants.value.find(q => q.id === quadrantId)
  if (quadrant) {
    const task = quadrant.tasks[index]
    const taskIndex = tasks.value.findIndex(t => t === task)
    if (taskIndex !== -1) {
      tasks.value[taskIndex].completed = true
      completedTaskName.value = task.name
      isCongratulationsModalOpen.value = true
    }
  }
}

const closeCongratulationsModal = () => {
  isCongratulationsModalOpen.value = false
  completedTaskName.value = ''
}

const openTaskDetail = (quadrantId, index) => {
  const quadrant = quadrants.value.find(q => q.id === quadrantId)
  if (quadrant) {
    taskToView.value = { ...quadrant.tasks[index] }
    currentQuadrantId.value = quadrantId
    isTaskDetailModalOpen.value = true
  }
}

const closeTaskDetailModal = () => {
  isTaskDetailModalOpen.value = false
  taskToView.value = null
  currentQuadrantId.value = null
}

const openEditModal = () => {
  taskToEdit.value = { ...taskToView.value }
  isEditModalOpen.value = true
  closeTaskDetailModal()
}

const completeTaskFromDetail = () => {
  const taskIndex = tasks.value.findIndex(t => t === taskToView.value)
  if (taskIndex !== -1) {
    tasks.value[taskIndex].completed = true
    completedTaskName.value = taskToView.value.name
    isCongratulationsModalOpen.value = true
    closeTaskDetailModal()
  }
}

const deleteTaskFromDetail = () => {
  const taskIndex = tasks.value.findIndex(t => t === taskToView.value)
  if (taskIndex !== -1) {
    tasks.value.splice(taskIndex, 1)
    closeTaskDetailModal()
  }
}


onMounted(() => {
  const savedTasks = localStorage.getItem('eisenhower-tasks')
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks)
  }
})

watch(tasks, (newTasks) => {
  localStorage.setItem('eisenhower-tasks', JSON.stringify(newTasks))
}, { deep: true })
</script>
<template>
  <div class="container-fluid">
    <div class="row vh-100">
      <div class="col-md-6 bg-light p-4">
        <h2>Nueva Tarea</h2>
        <TaskForm @addTask="addTask"/>
      </div>
      <div class="col-md-6 d-flex flex-column">
        <TaskQuadrant
          v-for="quadrant in quadrants"
          :key="quadrant.id"
          :title="quadrant.title"
          :tasks="quadrant.tasks"
          :bgColor="quadrant.color"
          @view-task="openTaskDetail(quadrant.id, $event)"
          @edit-task="editTask(quadrant.id, $event)"
          @delete-task="deleteTask(quadrant.id, $event)"
          @complete-task="completeTask(quadrant.id, $event)"
        />
      </div>
    </div>
    <TaskDetailModal
      :is-open="isTaskDetailModalOpen"
      :task="taskToView"
      @close="closeTaskDetailModal"
      @edit="openEditModal"
      @complete="completeTaskFromDetail"
      @delete="deleteTaskFromDetail"
    />
    <EditTaskModal
      :is-open="isEditModalOpen"
      :task="taskToEdit"
      @update-task="updateTask"
      @close="closeEditModal"
    />
    <CongratulationsModal
      :is-open="isCongratulationsModalOpen"
      :task-name="completedTaskName"
      @close="closeCongratulationsModal"
    />
  </div>
</template>