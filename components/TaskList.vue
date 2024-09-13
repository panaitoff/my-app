<template>
  <div>
    <h1>Task Manager</h1>
    <input v-model="task" placeholder="New task" />
    <button @click="addTask">Add Task</button>
    <ul>
      <li v-for="item in tasks" :key="item.id">{{ item.title }}</li>
    </ul>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';

const task = ref<string>('');
const tasks = ref<any[]>([]);

const fetchTasks = async () => {
  const res = await fetch('http://localhost:8000/tasks/');
  tasks.value = await res.json();
};

const addTask = async () => {
  if (task.value) {
    await fetch('http://localhost:8000/tasks/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ title: task.value }),
    });
    task.value = '';
    await fetchTasks();
  }
};

onMounted(fetchTasks);
</script>

<style scoped>
h1 {
  color: #333;
}
input {
  margin-right: 10px;
}
</style>
