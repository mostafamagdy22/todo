<template>
  <h1>Vue 3 Todo App</h1>
  <form @submit.prevent="addNewTodo">
    <label>New Todo</label>
    <input v-model="todo" name="newTodo" id="todoin">
    <button>Add New Todo</button>
  </form>
  <p v-if="noTasks">Tasks Not Found, PLEASE ADD TASKS.</p>
  <div v-else>
  <button @click="removeAll">Remove All</button>
  <button @click="markAllDone">Mark All Done</button>
  <button @click="unMark">Unmark All</button>
</div>
  <ul>
    <li v-for="(todo,indx) in todos" :key="todo.id">
      <h3 @click="toogleDone(todo)" :class="{done: todo.done}">{{todo.content}}</h3>
      <button @click="removeToDO(indx)">Remove Todo</button>
    </li>
  </ul>
</template>

<script>
import {ref} from 'vue';

export default {
  setup() {
    const todo = ref('');
    const todos = ref([]);
    const noTasks = ref(true);

    function addNewTodo() {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: todo.value
      });
      todo.value = '';
      document.getElementById('todoin').focus();
      localStorage.setItem('tasks',JSON.stringify(todos.value));
      noTasks.value = false;
    }

    function toogleDone(todo) {
      todo.done = !todo.done;
    }

    function removeAll() {
      todos.value = [];
      localStorage.removeItem('tasks');
      noTasks.value = true;
    }

    function markAllDone() {
      for (let i = 0; i < todos.value.length; i++) {
        todos.value[i].done = true;
      }
    }

    function unMark() {
      for (let i = 0; i < todos.value.length; i++) {
        todos.value[i].done = false;
      }
    }

    function removeToDO(indx) {
      todos.value.splice(indx,1);
      localStorage.setItem('tasks',JSON.stringify(todos.value));
      if (todos.value.length === 0) {
        noTasks.value = true;
      }
    }

    function storageData() {
      if (localStorage.getItem('tasks')) {
        todos.value = JSON.parse(localStorage.getItem('tasks'));
        noTasks.value = false;
      } else {
        noTasks.value = true;
      }
    }

    storageData();

    return {
      todo,
      todos,
      addNewTodo,
      toogleDone,
      removeAll,
      markAllDone,
      unMark,
      removeToDO,
      noTasks
    };
  }
}
</script>

<style>
body {
  font-family: sans-serif;
  padding-top: 1em;
  padding-bottom: 1em;
  font-size: 2em;
  width: 80%;
  margin: 0 auto;
}
input, textarea, button, p, div, section, article, select {
  display: 'block';
  width: 100%;
  font-family: sans-serif;
  font-size: 1em;
  margin: 0.5em;
}
.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}
</style>
