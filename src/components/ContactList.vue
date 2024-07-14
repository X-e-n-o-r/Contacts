<template>
    <transition-group name="fade" tag="div">
      <div v-for="contact in contacts" :key="contact.id" class="contact-item">
        <ContactItem
          :contact="contact"
          @edit="onEdit"
          @delete="onDelete"
        />
      </div>
    </transition-group>
  </template>
  
  <script setup lang="ts">
  import { PropType } from 'vue';
import ContactItem from './ContactItem.vue';
  
  interface Contact {
    id: number;
    name: string;
    phone: string;
    email: string;
  }
  
  defineProps({
    contacts: {
      type: Array as PropType<Contact[]>,
      required: true,
    },
  });
  
  const emit = defineEmits<{
    (e: 'edit', contact: Contact): void;
    (e: 'delete', id: number): void;
  }>();
  
  const onEdit = (contact: Contact) => {
    emit('edit', contact);
  };
  
  const onDelete = (id: number) => {
    emit('delete', id);
  };
  </script>
  
<style scoped>
  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
    opacity: 0;
  }
  .contact-item {
    margin-bottom: 1em;
  }
</style>
  