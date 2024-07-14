<template>
    <div class="contact">
      <div>{{ contact.name }}</div>
      <div>{{ contact.phone }}</div>
      <div>{{ contact.email }}</div>
      <button @click="editContact">Edit</button>
      <button @click="deleteContact">Delete</button>
    </div>
  </template>
  
  <script setup lang="ts">
  import { defineProps, defineEmits, PropType } from 'vue';
  
  interface Contact {
    id: number;
    name: string;
    phone: string;
    email: string;
  }
  
  const props = defineProps({
    contact: {
      type: Object as PropType<Contact>,
      required: true,
    },
  });
  
  const emit = defineEmits<{
    (e: 'edit', contact: Contact): void;
    (e: 'delete', id: number): void;
  }>();
  
  const editContact = () => {
    emit('edit', props.contact);
  };
  
  const deleteContact = () => {
    emit('delete', props.contact.id);
  };
  </script>

<style>
.contact {
    margin-top: 15px;
}
</style>