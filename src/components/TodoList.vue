<template>
  <div class="todo-list">
    <input class="task-input" v-model="newTask" @keyup.enter="addTask" placeholder="Add new task...">
    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" class="task-item">
        <span :class="{ completed: task.completed }">{{ task.text }}</span>
        <div class="task-buttons">
          <button class="edit-button" @click="editTask(index)">Edit</button>
          <button class="delete-button" @click="deleteTask(index)">Delete</button>
          <button class="complete-button" @click="toggleComplete(index)">
            {{ task.completed ? 'Mark Incomplete' : 'Mark Complete' }}
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from 'vue';

export default {
  setup() {
    const tasks = ref([]);
    const newTask = ref('');

    const loadTasks = () => {
      const savedTasks = JSON.parse(localStorage.getItem('tasks'));
      if (savedTasks) {
        tasks.value = savedTasks;
      }
    };

    const saveTasks = () => {
      localStorage.setItem('tasks', JSON.stringify(tasks.value));
    };

    const addTask = () => {
      if (newTask.value.trim() !== '') {
        tasks.value.push({ text: newTask.value, completed: false });
        newTask.value = '';
        saveTasks();
      }
    };

    const editTask = (index) => {
      const newText = prompt('Edit task:', tasks.value[index].text);
      if (newText !== null && newText.trim() !== '') {
        tasks.value[index].text = newText;
        saveTasks();
      }
    };

    const deleteTask = (index) => {
      if (confirm('Are you sure you want to delete this task?')) {
        tasks.value.splice(index, 1);
        saveTasks();
      }
    };

    const toggleComplete = (index) => {
      tasks.value[index].completed = !tasks.value[index].completed;
      saveTasks();
    };

    onMounted(loadTasks);

    onBeforeUnmount(() => {
      saveTasks();
    });

    return {
      tasks,
      newTask,
      addTask,
      editTask,
      deleteTask,
      toggleComplete
    };
  }
};
</script>

<style scoped>
.todo-list {
  max-width: 400px;
  margin: 0 auto;
}

.task-input {
  width: calc(100% - 40px);
  padding: 10px;
  font-size: 16px;
  margin-bottom: 10px;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.completed {
  text-decoration: line-through;
}

.task-buttons button {
  padding: 5px 10px;
  font-size: 14px;
  cursor: pointer;
}

.edit-button {
  background-color: #ffc107;
  color: #fff;
  border: none;
}

.delete-button {
  background-color: #dc3545;
  color: #fff;
  border: none;
}

.complete-button {
  background-color: #28a745;
  color: #fff;
  border: none;
}
</style>
