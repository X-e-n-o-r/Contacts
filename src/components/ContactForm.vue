<template>
    <form @submit.prevent="submitForm">
      <div class="label">
        <label>Name:</label>
        <input v-model="contact.name" required />
      </div>
      <div class="label">
        <label>Phone:</label>
        <input v-model="contact.phone" @input="validatePhone" required />
        <div v-if="phoneError" class="error">{{ phoneError }}</div>
      </div>
      <div class="label">
        <label>Email:</label>
        <input v-model="contact.email" @input="validateEmail" type="email" required />
        <div v-if="emailError" class="error">{{ emailError }}</div>
      </div>
      <button type="submit" :disabled="phoneError || emailError">{{ contact.id ? 'Update' : 'Add' }} Contact</button>
    </form>
  </template>
  
  <script setup lang="ts">
  import { ref, watch, defineProps, defineEmits } from 'vue';
  
  interface Contact {
    id: number;
    name: string;
    phone: string;
    email: string;
  }
  
  const props = defineProps<{ editContact: Contact | null }>();
  const emit = defineEmits<{
    (e: 'saveContact', contact: Contact): void;
  }>();
  
  const contact = ref<Contact>({
    id: 0,
    name: '',
    phone: '',
    email: '',
  });
  
  const phoneError = ref<string | null>(null);
  const emailError = ref<string | null>(null);
  
  watch(
    () => props.editContact,
    (newVal) => {
      if (newVal) {
        contact.value = { ...newVal };
      } else {
        contact.value = {
          id: 0,
          name: '',
          phone: '',
          email: '',
        };
      }
    },
    { immediate: true }
  );
  
  const validatePhone = () => {
    const phonePattern = /^\d{10}$/;
    if (!phonePattern.test(contact.value.phone)) {
      phoneError.value = 'Phone number must be 10 digits.';
    } else {
      phoneError.value = null;
    }
  };
  
  const validateEmail = () => {
    const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailPattern.test(contact.value.email)) {
      emailError.value = 'Invalid email address.';
    } else {
      emailError.value = null;
    }
  };
  
  const submitForm = () => {
    validatePhone();
    validateEmail();
    if (!phoneError.value && !emailError.value && contact.value.name) {
      emit('saveContact', { ...contact.value });
      contact.value = {
        id: 0,
        name: '',
        phone: '',
        email: '',
      };
    }
  };
  </script>

  
  
  <style>
  .error {
    color: red;
    font-size: 0.8em;
  }

  .label {
    margin-top: 15px;
  }
  </style>