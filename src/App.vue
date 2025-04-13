<script setup>
// Importing Composition API functions from Vue
import {
  ref,
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted
} from 'vue';


// ===================
// STATE / REACTIVE DATA
// ===================

// First component state
const message = ref('Initial Message'); // message displayed in header
const count = ref(0); // increments every second

// Second component state
const helloworld = ref('Hello World.'); // static message
const visible = ref(true); // condition to show/hide content
const status = ref('active'); // active | inactive | unknown


// ===================
// LIFECYCLE HOOKS - MOUNTING
// ===================

console.log('In setup - similar to created hook');

onBeforeMount(() => {
  console.log('onBeforeMount - about to render template');
});

onMounted(() => {
  console.log('onMounted - DOM is ready');

  // Start interval to increase count every second
  const timer = setInterval(() => {
    count.value++;
  }, 1000);

  // Clean up interval before unmount
  onBeforeUnmount(() => {
    clearInterval(timer);
  });
});


// ===================
// LIFECYCLE HOOKS - UPDATING
// ===================

onBeforeUpdate(() => {
  console.log('onBeforeUpdate - data changed, DOM not yet updated');
});

onUpdated(() => {
  console.log('onUpdated - DOM re-rendered');
});


// ===================
// LIFECYCLE HOOKS - UNMOUNTING
// ===================

onUnmounted(() => {
  console.log('onUnmounted - component is destroyed');
});


// ===================
// METHODS / FUNCTIONS
// ===================

function updateMessage() {
  message.value = 'Updated Message at ' + new Date().toLocaleTimeString();
}

function toggleStatus() {
  if (status.value === 'active') {
    status.value = 'inactive';
  } else if (status.value === 'inactive') {
    status.value = 'unknown';
  } else {
    status.value = 'active';
  }
}
</script>

<template>
  <div>
    <!-- ===================
         First Component
         =================== -->

    <h1>{{ message }}</h1>
    <button @click="updateMessage">Update Message</button>
    <div>Count: {{ count }}</div>

    <!-- ===================
         Second Component - v-if/v-else logic
         =================== -->

    <div v-if="visible">
      <h1 class="text-red-500">{{ helloworld }}</h1>
    </div>
    <div v-else>
      <h1 class="text-red-500">Hello world from not visible</h1>
    </div>

    <!-- ===================
         Status Toggle - if / else-if / else
         =================== -->

    <div>
      <button @click="toggleStatus">Toggle Status</button>

      <p v-if="status === 'active'">The status is active</p>
      <p v-else-if="status === 'inactive'">The status is inactive</p>
      <p v-else>The status is unknown</p>
    </div>
  </div>
</template>

<style scoped>
.text-red-500 {
  color: #ef4444;
}
</style>
