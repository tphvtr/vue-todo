<script setup lang="ts">
  import { RouterLink, RouterView } from 'vue-router'
  import { ref, onMounted } from 'vue'

  let todos = ref([]);
  let name = ref('');
  let _allTodos = [];

  function deleteTodo(i) {
    this.todos.splice(i, 1);
    this._allTodos = this.todos;
  }

  function addTodo(name) {
    this.todos.push({name, id: new Date().getTime()});
    this._allTodos = this.todos;
  }

  function search(query) {
    this.todos = this._allTodos.filter(item => item.name.toLocaleLowerCase().includes(query));
  }
</script>

<template>
  <header>
    <nav>
      <RouterLink to="/">
        <button>
          Home
        </button>
      </RouterLink>
      <RouterLink to="/about">
        <button>
          About
        </button>
      </RouterLink>
    </nav>
  </header>
  <RouterView />

  <h1>Todo List</h1>

  <div style="display: flex; margin-bottom: 20px;">
    <div>
      <input type="text" v-model="name" placeholder="ToDo name" maxlength="50">
      <p style="margin: 0;" :style="{color: name?.length < 50 ? 'grey' : 'red'}">{{name?.length}} / 50</p>
    </div>
    <button class="button" @click="() => addTodo(name)" :disabled="!name?.length">Create</button>
  </div>
  <input style="margin-bottom: 10px" type="text" placeholder="search..." @input="(e) => search(e.target.value)">

  <article v-for="(todo, i) in todos" class="todo-item">
    <span>{{ todo.name }}</span>
    <span @click="() => deleteTodo(i)">X</span>
  </article>
</template>

<style>
  * {
    box-sizing: border-box;
  }

  html, body {
    font-family: sans-serif, Arial, Helvetica;
    margin: 0;
    height: 100%;
    padding: 20px;
  }

  header {
    margin-bottom: 20px;
  }

  nav button {
    border: 1px solid #ccc;
    outline: none;
    border-radius: 4px;
    background-color: #fff;
    padding: 14px 24px;
    font-size: 14px;
    margin-right: 20px;
    cursor: pointer;
  }

  nav button:hover {
    background-color: #ccc;
  }

  .todo-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 500px;
    border: 1px solid #ccc;
    border-radius: 8px;
    padding: 18px;
    margin-bottom: 20px;
  }

  input {
    border-radius: 4px;
    height: 40px;
    border: 1px solid #ccc;
    margin-right: 20px;
  }

  .button {
    height: 40px;
    border: 1px solid #ccc;
    outline: none;
    border-radius: 4px;
    background-color: #fff;
    color: #363636;
    cursor: pointer;
  }
  .button:hover {
    background-color: #ccc;
  }
  .button:disabled {
    opacity: .5;
    pointer-events: none;
  }
</style>
