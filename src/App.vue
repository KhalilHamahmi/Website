<script setup>
import { ref, computed } from 'vue'

const taskInput = ref('')
const taskPrio  = ref(false)

const tasks = ref([
  { text: 'Rasen mähen',        done: false, high_priority: false },
  { text: 'Französisch lernen', done: false, high_priority: true },
  { text: 'Einkaufen',          done: false, high_priority: true },
  { text: 'Abfall rausbringen', done: false, high_priority: true },
])

function submit() {
  if (taskInput.value.trim() === '') return

  tasks.value.push({
    text: taskInput.value,
    done: false,
    high_priority: taskPrio.value,
  })

  taskInput.value = ''
  taskPrio.value = false
}

function removeTask(index) {
  tasks.value.splice(index, 1)
}

function getTaskStyle(task) {
  return {
    color: task.high_priority ? 'red' : 'black',
    fontWeight: task.high_priority ? 'bold' : 'normal',
    textDecoration: task.done ? 'line-through' : 'none'
  }
}

const sortedTasks = computed(() =>
    [...tasks.value].sort((a, b) => {
      if (a.high_priority === b.high_priority) return 0
      return a.high_priority ? -1 : 1
    })
)
</script>

<template>
  <div class="container">
    <h2>Aufgabenliste</h2>

    <p>
      {{ tasks.filter(t => t.done).length }} von {{ tasks.length }} Tasks sind erledigt
    </p>

    <div class="form-group">
      <label for="task">Neuer Task</label>
      <input class="form-control" id="task" v-model="taskInput">
    </div>

    <label for="prio">
      <input id="prio" type="checkbox" v-model="taskPrio">
      Hohe Priorität
    </label>

    <div class="form-actions">
      <button @click="submit">Task hinzufügen</button>
    </div>

    <ul>
      <li
          v-for="(item, j) in sortedTasks"
          :key="j"
          :style="getTaskStyle(item)"
      >
        <input type="checkbox" v-model="item.done">
        {{ item.text }}
        <button class="delete-button" @click="removeTask(j)">Löschen</button>
      </li>
    </ul>
  </div>
</template>

<style>
.container {
  margin: 20px auto;
  max-width: 400px;
  width: 100%;
}

ul {
  padding: 0;
  list-style: none;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 8px;
  padding: 4px 8px;
  border: 1px solid #ddd;
  border-radius: 6px;
}

.delete-btn {
  background: none;
  border: none;
  cursor: pointer;
  color: #888;
  font-size: 16px;
}

.delete-btn:hover {
  color: red;
}

.is-done {
  text-decoration: line-through;
}

.high-priority {
  font-weight: bold;
  color: red;
}

.form-group {
  display: block;
}

.form-group label {
  display: block;
  margin-bottom: 2px;
}

.form-control {
  width: 100%;
  padding: 2px 5px;
  height: 32px;
  margin-bottom: 5px;
}

.form-actions {
  display: block;
  margin-top: 1rem;
  margin-bottom: 2rem;
}
</style>
