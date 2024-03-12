<template>
  <div id="app">
    <h1>Todo List</h1>
    <TodoList />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue';

export default {
  components: {
    TodoList
  }
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 20px;
}

input {
  padding: 10px;
  font-size: 16px;
  margin-bottom: 10px;
}

button {
  margin-left: 5px;
}
</style>
