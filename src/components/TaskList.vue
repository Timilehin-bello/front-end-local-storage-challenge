<template>
  <div class="task-list-section">
    <ul>
      <li v-for="(task, index) in tasks" :key="task.id">
        <div class="task-details">
          <span><strong>ID:</strong> {{ task.id }}</span>
          <span><strong>Task:</strong> {{ task.task }}</span>
          <span><strong>Description:</strong> {{ task.description }}</span>
          <span><strong>Creation Date:</strong> {{ task.creationDate }}</span>
          <span><strong>Event Date:</strong> {{ task.eventDate }}</span>
        </div>
        <div class="edit-btns">
          <button @click="editTask(index)" class="edit-task-btn">Edit</button>
          <button @click="deleteTask(task.id)" class="delete-btn">
            Delete
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  tasks: Array,
});

const emit = defineEmits(["edit-task", "delete-task"]);

const editTask = (index) => {
  const task = props.tasks[index];
  // You can implement a more complex edit logic here, such as opening a modal
  const updatedTask = prompt("Edit task:", task.task);
  if (updatedTask !== null && updatedTask.trim() !== "") {
    emit("edit-task", task.id, { ...task, task: updatedTask });
  }
};

const deleteTask = (taskId) => {
  emit("delete-task", taskId);
};
</script>

<style scoped>
/* Task list section styling */
.task-list-section {
  margin-top: 20px;
}

/* Styling for each task item */
ul {
  list-style: none;
  padding: 0;
}

li {
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 15px;
  margin-bottom: 10px;
  transition: background-color 0.2s ease;
}

li:hover {
  background-color: #f0f0f0;
}

/* Task details styling */
.task-details span {
  display: block;
  margin-bottom: 8px;
  font-size: 14px;
}

/* Edit and delete button styling */
.edit-task-btn,
.delete-btn {
  padding: 6px 12px;
  margin-top: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: opacity 0.2s ease;
}

.edit-task-btn {
  background-color: #007bff;
  color: white;
}

.delete-btn {
  background-color: #dc3545;
  color: white;
}

.edit-task-btn:hover,
.delete-btn:hover {
  opacity: 0.9;
}
</style>
