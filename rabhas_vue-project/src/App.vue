<script setup>
import { ref } from "vue";

const header = ref("Shopping List App");
const editing = ref(false);
const items = ref([
  { label: "Rice", purchased: true, highpriority: true },
  { label: "Carrots", purchased: true, highpriority: false },
  { label: "Tofu", purchased: false, highpriority: true },
]);
const newItem = ref("");
const newItemPriority = ref(false);
const saveItem = () => {
  items.value.push({
    label: newItem.value,
    highPriority: newItemPriority.value,
  });
  newItem.value = "";
};
const doEdit = (e) => {
  editing.value = e;
  newItem.value = "";
};
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">
      Add Item
    </button>
  </div>
  <form class="add-item" v-if="editing" @submit.prevent="saveItem">
    <input v-model.trim="newItem" type="text" placeholder="Add an item" />
    <label>
      <input type="checkbox" v-model="newItemPriority" /> High Priority
    </label>
    <button :disabled="newItem.length < 3" class="btn btn-primary">
      Save Item
    </button>
  </form>
  <ul>
    <li
      v-for="{ label, purchased, highpriority } in items"
      class="static-class"
      :class="{ strikeout: purchased, priority: highpriority }"
    >
      {{ label }}
    </li>
  </ul>
  <p v-if="!items.length">No Item has been added</p>
</template>
