<template>
  <div class="container">
    <h1>To-Do List</h1>
    <TaskForm @add-task="addTask" :error-message="errorMessage" />
    <TaskList :tasks="tasks" @edit-task="editTask" @delete-task="deleteTask" />
  </div>
</template>

<script setup>
import { ref, watchEffect } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";

const tasks = ref(JSON.parse(localStorage.getItem("tasks")) || []);
const errorMessage = ref("");

watchEffect(() => {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
});

const addTask = (newTask) => {
  if (
    newTask.task.trim() === "" ||
    newTask.description.trim() === "" ||
    newTask.eventDate.trim() === ""
  ) {
    errorMessage.value = "Please fill in all fields.";
    return;
  }

  const currentDate = new Date().toISOString().split("T")[0];
  tasks.value.push({
    id: Date.now(), // unique identifier for each task
    task: newTask.task,
    description: newTask.description,
    creationDate: currentDate,
    eventDate: newTask.eventDate,
  });

  errorMessage.value = "";
};

const editTask = (taskId, updatedTask) => {
  const index = tasks.value.findIndex((task) => task.id === taskId);
  if (index !== -1) {
    tasks.value[index] = { ...tasks.value[index], ...updatedTask };
  }
};

const deleteTask = (taskId) => {
  tasks.value = tasks.value.filter((task) => task.id !== taskId);
};
</script>

<style scoped>
/* Container styles */
.container {
  max-width: 600px;
  margin: 30px auto;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

/* Title styles */
h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
  font-size: 24px;
}

/* Task form and list styles */
.input-section,
.task-list-section {
  margin-bottom: 20px;
}

/* Button styles */
button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s ease;
}

button:hover {
  opacity: 0.9;
}

/* Add task button */
.add-task-btn {
  background-color: #4caf50;
  color: white;
}

/* Edit and delete buttons */
.edit-task-btn {
  background-color: #007bff;
  color: white;
}

.delete-btn {
  background-color: #dc3545;
  color: white;
}

/* Error message styles */
.error-message {
  color: #ff0000;
  margin-top: 10px;
  font-size: 14px;
}
</style>
