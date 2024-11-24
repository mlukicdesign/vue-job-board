{{ /* 
  This is a simple Vue 3 app that fetches data from an API and displays it in a list.
  It also allows the user to add new tasks, delete tasks, and toggle the user status.
*/ }}

<script setup>
import { ref, onMounted } from 'vue'

const status = ref('active');
const tasks = ref([
  'Task One', 'Task Two', 'Task Three'
]);
const newTask = ref('');
const ToggleStatus = () => {
  status.value = status.value === 'active' ? 'pending' : 'active'
}

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
}

onMounted(async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts');
    const data = await res.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log(error);
  }
});

</script>



<template>
  <div id="app">
    <h1 class="text-4xl">{{ name }}</h1>
    <p v-if="status === 'active'">User is active</p>
    <p v-else-if="status === 'pending'">User is inactive</p>
    <br>

    <form @submit.prevent="addTask">
      <label for="newTask">Add Task</label>
      <input type="text" name="newTask" id="newTask" v-model="newTask">
      <button type="submit">Add Task</button>
    </form>

    <br>
    <h3>
      Tasks:
    </h3>
    <ul>
      <li v-for="(task, index) in tasks" :key="task">
        <span>{{ task }}</span>
        <button @click="deleteTask(index)">Delete Task</button>
      </li>
    </ul>
    <br>
    <button @click="ToggleStatus">Change Status</button>
  </div>
</template>
