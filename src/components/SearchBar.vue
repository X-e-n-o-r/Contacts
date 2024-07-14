<template>
    <div>
      <input v-model="query" @input="onInput" placeholder="Search by name" />
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, defineEmits } from 'vue';
  
  const query = ref<string>('');
  
  const emit = defineEmits<{
    (e: 'search', value: string): void;
  }>();
  
  const onInput = () => {
    emit('search', query.value);
  };
  </script>
  