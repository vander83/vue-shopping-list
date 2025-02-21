<script setup>
import { ref, onMounted } from 'vue';

onMounted(() => {
  getItemsFromStorage();
});

const items = ref([]);

const uniqueId = ref('');
const newItemName = ref('');

const generateUniqueId = () => {
  uniqueId.value = Date.now();
};

const clearInput = () => {
  newItemName.value = '';
};

const getItemsFromStorage = () => {
  const itemsFromStorage = localStorage.getItem('items');
  if (itemsFromStorage) {
    items.value = JSON.parse(itemsFromStorage);
  }
};

const updateLocalStorage = () => {
  localStorage.setItem('items', JSON.stringify(items.value));
};

const addItem = (newItemName) => {
  generateUniqueId();
  items.value.push({ id: uniqueId.value, name: newItemName, checked: false });
  console.log(items.value);
  updateLocalStorage();
  getItemsFromStorage();
  clearInput();
};

const checkItem = (id) => {
  const item = items.value.find((item) => item.id === id);
  item.checked = !item.checked;
  updateLocalStorage();
  getItemsFromStorage();
};

const deleteItem = (id) => {
  items.value = items.value.filter((item) => item.id !== id);
  updateLocalStorage();
  getItemsFromStorage();
};

const clearList = () => {
  items.value = [];
  updateLocalStorage();
  getItemsFromStorage();
};
</script>

<template>
  <div class="shopping-list">
    <h1>Shopping List</h1>

    <form @submit.prevent="addItem(newItemName)" class="add-item-container">
      <input type="text" placeholder="Add item" v-model="newItemName" />
      <button class="button">Add</button>
    </form>
    
    <!-- <div class="filter-container">
      <input type="text" placeholder="Filter items" />
    </div> -->
    
    <TransitionGroup name="list" tag="ul" class="items">
      <li class="item" v-for="item in items" :key="item.id">
        <input type="checkbox" name="check" id="check" v-model="item.checked" @click="checkItem(item.id)"/>
        <span class="item-name">{{ item.name }}</span>
        <button class="button delete-button" @click="deleteItem(item.id)">Delete</button>
      </li>
    </TransitionGroup>

    <div class="clear-all-container" @click="clearList">
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
  grid-template-rows: auto auto 1fr auto;
  align-content: start;
  gap: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #262b30;
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
  background-color: #394149;
  color: #fff;
}

input[type="checkbox"] {
  width: 20px;
  height: 20px;
}

::placeholder {
  color: #e4e4e4;
}

.items {
  display: grid;
  align-content: start;
  gap: 10px;
  overflow-y: auto;
  overflow-x: hidden;
}

.item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  gap: 10px;
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

/* Animations & Transitions */
.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
