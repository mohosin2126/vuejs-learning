<!-- NewComponent.vue -->
<script setup>
import { ref, computed, watch } from 'vue';
import PracticeForm from"../form/index.vue"
// Component state
const title = ref('Practice');
const items = ref([
  { id: 1, text: 'First item' },
  { id: 2, text: 'Second item' },
  { id: 3, text: 'Third item' }
]);
const newItemText = ref('');
const searchQuery = ref('');

// Computed property
const filteredItems = computed(() => {
  return items.value.filter(item =>
      item.text.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

// Methods
function addItem() {
  if (newItemText.value.trim()) {
    const id = items.value.length > 0
        ? Math.max(...items.value.map(item => item.id)) + 1
        : 1;

    items.value.push({
      id,
      text: newItemText.value
    });
    newItemText.value = '';
  }
}

function removeItem(id) {
  const index = items.value.findIndex(item => item.id === id);
  if (index !== -1) {
    items.value.splice(index, 1);
  }
}

// Watch for changes
watch(searchQuery, (newValue) => {
  console.log(`Searching for: ${newValue}`);
});
</script>

<template>
  <div class="my-component bg-white p-6 rounded-lg shadow-md">
    <h2 class="text-xl font-bold text-indigo-600 mb-4">{{ title }}</h2>

    <!-- Search functionality -->
    <div class="mb-4">
      <input
          v-model="searchQuery"
          placeholder="Search items..."
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
      />
    </div>

    <!-- Add new item form -->
    <div class="flex mb-4">
      <input
          v-model="newItemText"
          placeholder="Add new item"
          class="flex-1 px-3 py-2 border border-gray-300 rounded-l-md focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500"
          @keyup.enter="addItem"
      />
      <button
          @click="addItem"
          class="bg-indigo-500 hover:bg-indigo-600 text-white px-4 py-2 rounded-r-md font-medium transition duration-200"
      >
        Add
      </button>
    </div>

    <!-- Items list -->
    <ul v-if="filteredItems.length > 0" class="space-y-2">
      <li
          v-for="item in filteredItems"
          :key="item.id"
          class="flex justify-between items-center p-3 bg-gray-50 rounded-md hover:bg-gray-100 transition duration-200"
      >
        <span>{{ item.text }}</span>
        <button
            @click="removeItem(item.id)"
            class="text-red-500 hover:text-red-700 transition duration-200"
        >
          Remove
        </button>
      </li>
    </ul>

    <p v-else class="text-gray-500 text-center py-4">
      No items found. Add some items or try a different search.
    </p>

    <PracticeForm/>

  </div>
</template>