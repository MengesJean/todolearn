<template>
    <div class="todo-list">
      <input v-model="newTodoText" @keyup.enter="addTodo" class="input" placeholder="Ajouter une tâche" />
      <div class="pt-3">
        <input type="radio" id="all" value="all" v-model="filter"/><label for="all" class="ps-1 pe-3">Toutes</label>
        <input type="radio" id="completed" value="completed" v-model="filter"/><label for="completed" class="ps-1 pe-3">Terminées</label>
        <input type="radio" id="notCompleted" value="notCompleted" v-model="filter"/><label for="notCompleted" class="ps-1 pe-3">Non terminées</label>
      </div>
      <ul v-if="filteredTodos.length">
        <TodoItem
          v-for="todo in filteredTodos"
          :key="todo.id"
          :todo="todo"
          @delete="removeTodo"
          @toggle="toggleTodoCompletion"
        />
      </ul>
      <h1 v-else class="py-2 px-4">Aucune todo</h1>
    </div>
  </template>
  
<script setup lang="ts">
import { ref, watch, computed } from 'vue';
import TodoItem from './TodoItem.vue';
import { Todo } from '../types';

const todos = ref<Todo[]>([]);
const newTodoText = ref('');
const filter = ref('all');

watch(todos, (newTodos) => {
    console.log(newTodos)
}, {deep: true})

const addTodo = () => {
    if (newTodoText.value.trim()) {
        const newTodo: Todo = {
            id: Date.now(),
            text: newTodoText.value,
            completed: false,
        };
        todos.value.push(newTodo);
        newTodoText.value = '';
    }
};

const removeTodo = (id: number) => {
    todos.value = todos.value.filter((todo) => todo.id !== id);
};

const toggleTodoCompletion = (id: number) => {
    const todo = todos.value.find((todo) => todo.id === id);
    if (todo) {
        todo.completed = !todo.completed;
    }
};

const filteredTodos = computed(() => {
  switch (filter.value) {
    case 'completed':
      return todos.value.filter((todo) => todo.completed);
    case 'notCompleted':
      return todos.value.filter((todo) => !todo.completed);
    default:
      return todos.value;
  }
});

</script>
  