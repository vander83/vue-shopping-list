<script setup>
import { ref, onMounted } from 'vue';
import ListItem from './components/ListItem.vue';

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
  const itemsFromStorage = localStorage.getItem('shoppingItems');
  if (itemsFromStorage) {
    items.value = JSON.parse(itemsFromStorage);
  }
};

const updateLocalStorage = () => {
  localStorage.setItem('shoppingItems', JSON.stringify(items.value));
};

const addItem = (newItemName) => {
  generateUniqueId();
  items.value.unshift({ id: uniqueId.value, name: newItemName, checked: false });
  // console.log(items.value);
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
    <header class="header">
      <div class="logo-container">
        <div class="logo">
          <img src="/assets/icons/shopping-cart-icon.png" alt="logo">
        </div>
        <h1>Shopping List</h1>
      </div>

      <form @submit.prevent="addItem(newItemName)" class="add-item-container">
        <input type="text" placeholder="Add item" v-model="newItemName" />
        <button class="icon-button add-button" :disabled="!newItemName" type="submit">
          <img src="/assets/icons/plus-icon-2.svg" alt="add icon">
        </button>
      </form>
    </header>

    
    <!-- <div class="filter-container">
      <input type="text" placeholder="Filter items" />
    </div> -->
    
    <TransitionGroup name="list" tag="ul" class="items">
      <!-- <li class="item" v-for="item in items" :key="item.id">
        <input type="checkbox" name="check" id="check" v-model="item.checked" @click="checkItem(item.id)"/>
        <span class="item-name">{{ item.name }}</span>
        <button class="icon-button delete-button" @click="deleteItem(item.id)">
          <img src="/assets/icons/plus-icon-2.svg" alt="delete icon">
        </button>
      </li> -->
      <li class="item" v-for="item in items" :key="item.id">
        <ListItem :item="item" @itemChecked="checkItem" @deleteClicked="deleteItem" />
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
  height: calc(100vh - 40px);
  margin: 0 auto;
  display: grid;
  grid-template-rows: auto auto 1fr auto;
  align-content: start;
  gap: 10px;
  border-radius: 10px;
  background-color: var(--bg-color-light);
}

.header {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 20px 20px 20px;
  display: grid;
  gap: 20px;
  background-color: var(--primary-color);
  background-color: #282828;
  border-radius: 10px 10px 0 0;
}

.logo-container {
  display: flex;
  align-items: center;
  gap: 10px;
}

.logo-container h1 {
  font-weight: 600;
  color: #fff;
}

.logo {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: #fff;
  padding: 8px 8px 6px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo img {
  width: 100%;
  height: auto;
}

.add-item-container, .filter-container, .clear-all-container {
  display: flex;
  align-items: center;
  gap: 10px;
  width: 100%;
}

input {
  width: 100%;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #fff;
  background-color: var(--bg-color-light);
  color: var(--text-color);
}

::placeholder {
  color: var(--text-color);
}

.add-button img {
  filter: invert(1);
}

.add-button {
  background-color: var(--primary-color);
  border: 2px solid var(--primary-color);
  border-radius: 5px;
  transition: all 0.2s ease;
}

.add-button:hover {
  background-color: transparent;
}

.add-button:disabled {
  background-color: #666;
  border-color: #666;
}

.add-button img {
  background: transparent;
}

.items {
  display: grid;
  align-content: start;
  gap: 10px;
  overflow-y: auto;
  overflow-x: hidden;
  padding: 10px 20px;
}

.clear-all-container {
  align-self: end;
  padding: 10px 20px 20px;
}

.clear-all-container  button {
  width: 100%;
  background-color: var(--danger-color);
  color: #fff;
  border-color: var(--danger-color);
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

/* Media queries */
@media (max-width: 768px) {
  .shopping-list {
    height: calc(100vh - 20px);
  }
}
</style>
