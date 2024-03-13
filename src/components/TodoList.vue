<template>
  <div class="todo-list">
    <input class="task-input" v-model="newTask" @keyup.enter="addNewTask" placeholder="Add new task...">
    <ul class="task-list">
      <li v-for="(task, index) in taskList" :key="index" class="task-item">
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
  name: 'TodoList',
  setup() {
    const taskList = ref([]);
    const newTask = ref('');

    const loadTasks = () => {
      const savedTasksList = JSON.parse(localStorage.getItem('taskList'));
      if (savedTasksList) {
        taskList.value = savedTasksList;
      }
    };

    const saveTasksList = () => {
      localStorage.setItem('taskList', JSON.stringify(taskList.value));
    };

    const addNewTask = () => {
      if (newTask.value.trim() !== '') {
        taskList.value.push({ text: newTask.value, completed: false });
        newTask.value = '';
        saveTasksList();
      }
    };

    const editTask = (index) => {
      const newText = prompt('Edit task:', taskList.value[index].text);
      if (newText !== null && newText.trim() !== '') {
        taskList.value[index].text = newText;
        saveTasksList();
      }
    };

    const deleteTask = (index) => {
      if (confirm('Are you sure you want to delete this task?')) {
        taskList.value.splice(index, 1);
        saveTasksList();
      }
    };

    const toggleComplete = (index) => {
      taskList.value[index].completed = !taskList.value[index].completed;
      saveTasksList();
    };

    onMounted(loadTasks);

    onBeforeUnmount(() => {
      saveTasksList();
    });

    return {
      taskList,
      newTask,
      addNewTask,
      editTask,
      deleteTask,
      toggleComplete
    };
  }
};
</script>

<style scoped>
.todo-list {
  max-width: 620px;
  margin: 0 auto;
}

.task-input {
  width: calc(100% - 240px);
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
   width: 60px; 
 background-color: #3f51b5;
  color: #fff;
  border: none;
}

.delete-button {
   width: 60px; 
background-color: #f44336;
  color: #fff;
  border: none;
}

.complete-button {
   width: 124px;
  background-color: #28a745;
  color: #fff;
  border: none;
}
</style>
