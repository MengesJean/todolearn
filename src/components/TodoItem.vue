<template>
    <div class="todo-item">
      <span :class="{ completed: todo.completed }" @click="toggleCompletion">{{ todo.text }}</span>
      <button @click="deleteTodo" class="delete">X</button>
    </div>
  </template>
  
  <script setup lang="ts">
  import { defineProps, defineEmits } from 'vue';
  import { Todo } from '../types';
  
  const props = defineProps<{
    todo: Todo;
  }>();
  
  const emits = defineEmits(['delete', 'toggle']);
  
  const toggleCompletion = () => {
    emits('toggle', props.todo.id);
  };
  
  const deleteTodo = () => {
    emits('delete', props.todo.id);
  };
  </script>
  
  <style>
  .completed {
    @apply text-gray-400 line-through;
  }
  .delete {
    @apply text-red-500;
  }
  
  </style>
  