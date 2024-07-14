<template>
  <div id="app">
    <h1>Contact Management</h1>
    <SearchBar @search="searchContact" />
    <ContactForm @saveContact="saveContact" :editContact="editContactData" />
    <ContactList
      :contacts="filteredContacts"
      @edit="editContact"
      @delete="deleteContact"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import SearchBar from './components/SearchBar.vue';
import ContactForm from './components/ContactForm.vue';
import ContactList from './components/ContactList.vue';

interface Contact {
  id: number;
  name: string;
  phone: string;
  email: string;
}

const contacts = ref<Contact[]>(JSON.parse(localStorage.getItem('contacts') || '[]'));
const searchQuery = ref<string>('');
const editContactData = ref<Contact | null>(null);

const saveContact = (contact: Contact) => {
  if (editContactData.value) {
    const index = contacts.value.findIndex(c => c.id === contact.id);
    contacts.value[index] = contact;
    editContactData.value = null;
  } else {
    contact.id = Date.now();
    contacts.value.push(contact);
  }
  localStorage.setItem('contacts', JSON.stringify(contacts.value));
};

const editContact = (contact: Contact) => {
  editContactData.value = { ...contact };
};

const deleteContact = (id: number) => {
  contacts.value = contacts.value.filter(contact => contact.id !== id);
  localStorage.setItem('contacts', JSON.stringify(contacts.value));
};

const searchContact = (query: string) => {
  searchQuery.value = query;
};

const filteredContacts = computed(() =>
  contacts.value.filter(contact =>
    contact.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
);
</script>