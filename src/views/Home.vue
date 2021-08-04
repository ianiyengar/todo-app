<template>
  <div class="home">
    <h1>New To Do Item</h1>
    <div>
      Name:
      <input type="text" v-model="newTodo.name" />
      Item:
      <input type="text" v-model="newTodo.item" />

      <button v-on:click="createTodo()">Create To Do</button>
    </div>
    <h1>All To do's</h1>
    <div v-for="todo in todos" v-bind:key="todo.id">
      <h2>{{ todo.name }}</h2>
      <p>Name: {{ todo.name }}</p>
      <p>Item: {{ todo.item }}</p>
    </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      todos: [],
      newTodo: {},
    };
  },
  created: function () {
    this.indexTodos();
  },
  methods: {
    indexTodos: function () {
      axios.get("/todos").then((response) => {
        console.log("todos index", response);
        this.todos = response.data;
      });
    },
    createTodo: function () {
      var params = {
        name: this.newTodo.name,
        item: this.newTodo.item,
      };
      axios
        .post("/todos", params)
        .then((response) => {
          console.log("todos create", response);
          this.todos.push(response.data);
          this.newTodo = {};
        })
        .catch((error) => {
          console.log("todos create error", error.response);
        });
    },
  },
};
</script>
