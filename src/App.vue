<script setup>
import { ref } from "vue";

const header = ref("My Todos");
const todos = ref([]);
// const reversedTodos
const newTodo = ref("");
const highPriority = ref(false);
const editing = ref(false);
const addTodo = () => {
  todos.value.unshift({
    id: todos.value.length + 1,
    label: newTodo.value,
    highPrio: highPriority.value,
    completed: false,
  });
  highPriority.value = false;
  newTodo.value = "";
};
const edit = (e) => {
  editing.value = e;
};
const toggleComplete = (item) => {
  item.completed = !item.completed;
};
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button
      v-if="editing"
      class="btn btn-cancel btn-header"
      @click="edit(false)"
    >
      Cancel
    </button>
    <button v-else class="btn btn-primary btn-header" @click="edit(true)">
      Add Items
    </button>
  </div>

  <br />
  <form class="add-item-form" v-if="editing" @submit.prevent="addTodo()">
    <input type="text" placeholder="What do you need to do" v-model="newTodo" />
    <label>
      <input type="checkbox" v-model="highPriority" /> High Priority
    </label>
    <button class="btn btn-primary" :disabled="newTodo.length < 5">
      Add Todo
    </button>
  </form>
  <br />
  <ul>
    <li
      v-for="todo in todos"
      :class="{ strikeout: todo.completed, priority: todo.highPrio }"
      :keys="todo.id"
      @click="toggleComplete(todo)"
    >
      {{ todo.label }}
    </li>
  </ul>
  <p v-if="!todos.length">Nothing to do!</p>
</template>
