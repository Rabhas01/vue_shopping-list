<script setup>
import { ref, computed } from "vue";

const header = ref("Shopping List App");
const characaterCount = computed(() => {
  return newItem.value.length
})
const editing = ref(false);
const items = ref([
  { label: "Rice", purchased: true, highpriority: true },
  { label: "Carrots", purchased: false, highpriority: false },
  { label: "Tofu", purchased: false, highpriority: true },
]);
const newItem = ref("");
const newItemPriority = ref(false);
const saveItem = () => {
  items.value.push({
    label: newItem.value,
    highPriority: newItemPriority.value,
  });
  newItem.value = ""
  newItemPriority.value = ""

};
const doEdit = (e) => {
  editing.value = e;
  newItem.value = "";
  newItemPriority.value = ""
};
const togglePurchased = (item) => {
  item.purchased = !item.purchased
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">
      Cancel
    </button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">
      Add Item
    </button>
  </div>
  <form class="add-item" v-if="editing" @submit.prevent="saveItem">
    <input v-model.trim="newItem" type="text" placeholder="Add an item" />
    <label>
      <input type="checkbox" v-model="newItemPriority" /> 
      High Priority
    </label>
    <button :disabled="newItem.length < 3" class="btn btn-primary">
      Save Item
    </button>
  </form>
  <p class="counter">
    {{characaterCount}}/250
  </p>
  <ul>
    <li
      v-for="(item, index) in items"
      @click="togglePurchased(item)"
      class="static-class"
      :class="{
         strikeout: item.purchased,
         priority: item.highpriority 
      }"
    >
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">
  No Item has been added
  </p>
</template>
