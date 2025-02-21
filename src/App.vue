<script setup>
import { ref } from 'vue';

const items = ref([]);

const uniqueId = ref('');
const newItemName = ref('');

const generateUniqueId = () => {
  uniqueId.value = Date.now();
};

const clearInput = () => {
  newItemName.value = '';
};

const addItem = (newItemName) => {
  generateUniqueId();
  console.log(`id:${uniqueId.value} name: ${newItemName}`);
  clearInput();
};
</script>

<template>
  <div class="shopping-list">
    <h1>Shopping List</h1>

    <form @submit.prevent="addItem(newItemName)" class="add-item-container">
      <input type="text" placeholder="Add item" v-model="newItemName" />
      <button class="button">Add</button>
    </form>

    <div class="filter-container">
      <input type="text" placeholder="Filter items" />
    </div>

    <ul class="items">
      <li class="item" v-for="item in items" :key="item.id">
        <span class="item-name">{{ item.name }}</span>
        <button class="button delete-button">Delete</button>
      </li>
    </ul>

    <div class="clear-all-container">
      <button class="button">Clear All</button>
    </div>
  </div>
</template>

<style scoped>
.shopping-list {
  max-width: 600px;
  height: calc(100vh - 80px);
  margin: 0 auto;
  padding: 20px;
  display: grid;
  grid-template-rows: auto auto auto 1fr auto;
  align-content: start;
  gap: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.add-item-container, .filter-container, .clear-all-container {
  display: flex;
  gap: 10px;
  width: 100%;
}

input {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #fff;
}

.button {
  padding: 10px 20px;
  border-radius: 5px;
  border: 1px solid #fff;
  background-color: transparent;
  color: #fff;
}

.button:hover {
  background-color: #fff;
  color: #333;
}

.items {
  display: grid;
  align-content: start;
  gap: 10px;
  overflow-y: auto;
}

.item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.clear-all-container {
  align-self: end;
}

.clear-all-container  button {
  width: 100%;
}
</style>
