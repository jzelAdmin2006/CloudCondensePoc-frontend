<script lang="ts">
import { defineComponent, ref } from 'vue';
import { addCloudStorage, getAllCloudStorages } from './api/requests.ts';

export default defineComponent({
  name: 'App',
  setup() {
    const storages = ref<any[]>([]);
    const newStorage = ref({
      name: '',
      type: '',
      username: '',
      password: ''
    });

    const fetchStorages = async () => {
      storages.value = await getAllCloudStorages();
    };

    const addNewStorage = async () => {
      await addCloudStorage(newStorage.value);
      fetchStorages();
    };

    fetchStorages();

    return { storages, newStorage, addNewStorage };
  }
});
</script>

<template>
  <div>
    <h1>CloudCondense</h1>

    <h2>Add New Storage</h2>
    <input v-model="newStorage.name" placeholder="Name" />
    <select v-model="newStorage.type">
      <!-- Todo: Get storage types from backend -->
      <option value="Google Drive">Google Drive</option>
      <option value="TYPE2">Type 2</option>
      <!-- usw. -->
    </select>
    <input v-model="newStorage.username" placeholder="Username" />
    <input v-model="newStorage.password" placeholder="Password" />
    <button @click="addNewStorage">Add</button>
    
    <h2>Cloud Storages</h2>
    <ul>
      <li v-for="storage in storages" :key="storage.id">
        {{ storage.name }} ({{ storage.type }}) with user {{ storage.username }}
      </li>
    </ul>
  </div>
</template>
