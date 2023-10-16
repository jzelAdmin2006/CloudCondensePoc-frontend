<script lang="ts">
import { defineComponent, ref } from "vue";
import {
  addCloudStorage,
  getAllCloudStorages,
  getCloudStorageTypes,
} from "./api/requests.ts";

export default defineComponent({
  name: "App",
  setup() {
    const storages = ref<any[]>([]);
    const newStorage = ref({
      name: "",
      type: "",
      username: "",
      password: "",
    });
    const storageTypes = ref<string[]>([]);

    const fetchStorages = async () => {
      storages.value = await getAllCloudStorages();
    };

    const addNewStorage = async () => {
      await addCloudStorage(newStorage.value);
      fetchStorages();
    };

    fetchStorages();
    const fetchStorageTypes = async () => {
      storageTypes.value = await getCloudStorageTypes();
    };

    fetchStorageTypes();

    return { storages, storageTypes, newStorage, addNewStorage };
  },
});
</script>

<template>
  <div>
    <h1>CloudCondense</h1>

    <h2>Add New Storage</h2>
    <input v-model="newStorage.name" placeholder="Name" />
    <select v-model="newStorage.type">
      <option v-for="type in storageTypes" :key="type" :value="type">
        {{ type }}
      </option>
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
