<template>
  <div class="input-section">
    <form @submit.prevent="submitTask">
      <label for="task">Task:</label>
      <input
        type="text"
        id="task"
        v-model="taskInput"
        placeholder="Enter task..."
        required
      />
      <label for="description">Description:</label>
      <input
        type="text"
        id="description"
        v-model="descriptionInput"
        placeholder="Enter description..."
        required
      />
      <label for="event-date">Date:</label>
      <input type="date" id="event-date" v-model="eventDateInput" required />
      <button type="submit">Add Task</button>
      <p class="error-message">{{ errorMessage }}</p>
    </form>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits } from "vue";

defineProps({
  errorMessage: String,
});

const emit = defineEmits(["add-task"]);

const taskInput = ref("");
const descriptionInput = ref("");
const eventDateInput = ref("");

const submitTask = () => {
  const newTask = {
    task: taskInput.value,
    description: descriptionInput.value,
    eventDate: eventDateInput.value,
  };

  emit("add-task", newTask);
  taskInput.value = "";
  descriptionInput.value = "";
  eventDateInput.value = "";
};
</script>

<style scoped>
/* Input section styling */
.input-section {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}

/* Label styling */
label {
  font-weight: bold;
  margin-bottom: 5px;
}

/* Input and button styling */
input[type="text"],
input[type="date"],
button {
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
  font-size: 16px;
}

/* Styling for text and date inputs */
input[type="text"],
input[type="date"] {
  width: 100%;
}

/* Add task button styling */
button[type="submit"] {
  background-color: #4caf50;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
}

button[type="submit"]:hover {
  background-color: #45a049;
}

/* Error message styling */
.error-message {
  color: #ff0000;
  margin-top: 10px;
  font-size: 14px;
}
</style>
