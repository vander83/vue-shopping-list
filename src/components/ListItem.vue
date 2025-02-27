<script setup>
const emits = defineEmits(['itemChecked', 'deleteClicked']);
const props = defineProps(['item']);

</script>

<template>
    <div class="item">
        <input type="checkbox" name="check" id="check" v-model="item.checked" @click="$emit('itemChecked', item.id)"/>
        <span class="item-name">{{ item.name }}</span>
        <button class="icon-button delete-button" @click="$emit('deleteClicked', item.id)">
            <img src="/assets/icons/plus-icon-2.svg" alt="delete icon">
        </button>
    </div>
</template>

<style scoped>
.item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  gap: 10px;
  align-items: center;
  padding: 10px;
  /* border: 1px solid #ccc; */
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

input[type="checkbox"] {
  width: 20px;
  height: 20px;
}

.delete-button img {
  transform: rotate(45deg);
  width: 20px;
  height: 20px;
  filter: brightness(0) saturate(100%) invert(70%) sepia(43%) saturate(5502%) hue-rotate(321deg) brightness(84%) contrast(104%);
}

.delete-button:hover img {
  transform: scale(1.1) rotate(45deg);
}
</style>
