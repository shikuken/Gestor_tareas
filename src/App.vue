<template>
  <div class="container">
    <h1>Gestor de Tareas</h1>
    <input v-model="newTask" placeholder="Escribe el nombre de la tarea" @keyup.enter="addTask" />
    <button @click="addTask">Agregar</button>
    <hr />

    <div v-if="tasks.length === 0">
      <p>No hay tareas asignadas.</p>
    </div>
    
    <div class="kanban">
      <div class="column">
        <h2>To do</h2>
        <div v-for="task in tasksByStatus('todo')" :key="task.id" class="task todo">
          {{ task.name }}
          <button @click="moveTask(task, 'doing')">→</button>
        </div>
      </div>
      <div class="column">
        <h2>Doing</h2>
        <div v-for="task in tasksByStatus('doing')" :key="task.id" class="task doing">
          {{ task.name }}
          <button @click="moveTask(task, 'done')">→</button>
        </div>
      </div>
      <div class="column">
        <h2>Done</h2>
        <div v-for="task in tasksByStatus('done')" :key="task.id" class="task done">
          {{ task.name }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTask = ref('')
const tasks = ref([])

const addTask = () => {
  const name = newTask.value.trim()
  if (name) {
    tasks.value.push({ id: Date.now(), name, status: 'todo' })
    newTask.value = ''
  }
}

const moveTask = (task, nextStatus) => {
  task.status = nextStatus
}

const tasksByStatus = (status) => tasks.value.filter(t => t.status === status)
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: 40px auto;
  font-family: Arial, sans-serif;
}
.kanban {
  display: flex;
  gap: 16px;
}
.column {
  flex: 1;
  background: #f4f4f4;
  padding: 12px;
  border-radius: 8px;
  min-height: 200px;
}
.task {
  margin-bottom: 10px;
  padding: 8px;
  border-radius: 4px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.todo { border: 2px solid #2196f3; }
.doing { border: 2px solid #4caf50; }
.done { border: 2px solid #f44336; }
button {
  margin-left: 8px;
  padding: 4px 8px;
}
</style>

