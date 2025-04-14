<script setup>
//importing another component
import Practice from "./components/practice/index.vue"

// Importing Composition API functions from Vue
import {
  ref,
  reactive,
  computed,
  watch,
  provide,
  inject,
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
const count = ref(0); // counter value that can be incremented/decremented
const lastUpdated = ref(null); // timestamp of last update

// Second component state
const helloworld = ref('Hello World.'); // static message
const visible = ref(true); // condition to show/hide content
const status = ref('active'); // active | inactive | unknown

// New features from cheat sheet
const name = ref(''); // for v-model demonstration
const isAdmin = ref(false); // for v-if demonstration
const url = ref('https://vuejs.org'); // for dynamic attribute binding
const activeColor = ref('#42b983'); // for dynamic style binding

// Reactive object example
const user = reactive({
  name: 'Vue Developer',
  age: 25,
  email: 'dev@example.com'
});

// v-for demo array
const items = ref([
  { id: 1, name: 'Item 1', completed: false },
  { id: 2, name: 'Item 2', completed: true },
  { id: 3, name: 'Item 3', completed: false }
]);

// Dynamic class demo
const isActive = ref(true);
const hasError = ref(false);

// Computed property example
const activeItemsCount = computed(() => {
  return items.value.filter(item => !item.completed).length;
});

// ===================
// LIFECYCLE HOOKS - MOUNTING
// ===================

console.log('In setup - similar to created hook');

onBeforeMount(() => {
  console.log('onBeforeMount - about to render template');
});

onMounted(() => {
  console.log('onMounted - DOM is ready');
  // Set initial timestamp
  lastUpdated.value = new Date().toLocaleTimeString();
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

onBeforeUnmount(() => {
  console.log('onBeforeUnmount - component will be destroyed');
});

onUnmounted(() => {
  console.log('onUnmounted - component is destroyed');
});


// ===================
// METHODS / FUNCTIONS
// ===================

function updateMessage() {
  message.value = 'Updated Message at ' + new Date().toLocaleTimeString();
  lastUpdated.value = new Date().toLocaleTimeString();
}

function incrementCount() {
  count.value++;
  lastUpdated.value = new Date().toLocaleTimeString();
}

function decrementCount() {
  count.value--;
  lastUpdated.value = new Date().toLocaleTimeString();
}

function resetCount() {
  count.value = 0;
  lastUpdated.value = new Date().toLocaleTimeString();
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

// New methods from cheat sheet
function toggleAdmin() {
  isAdmin.value = !isAdmin.value;
}

function toggleVisibility() {
  visible.value = !visible.value;
}

function addItem() {
  const id = items.value.length + 1;
  items.value.push({
    id,
    name: `Item ${id}`,
    completed: false
  });
}

function removeItem(id) {
  const index = items.value.findIndex(item => item.id === id);
  if (index !== -1) {
    items.value.splice(index, 1);
  }
}

function toggleItemCompletion(id) {
  const item = items.value.find(item => item.id === id);
  if (item) {
    item.completed = !item.completed;
  }
}

function handleSubmit() {
  console.log('Form submitted with name:', name.value);
  alert(`Form submitted: ${name.value}`);
}

function loadOnce() {
  console.log('This will only execute once');
  alert('This alert will only show once per component mount');
}

function hoverFunc() {
  console.log('Mouse over event triggered');
}

// Example of watch
watch(name, (newValue, oldValue) => {
  console.log(`Name changed from ${oldValue} to ${newValue}`);
});

// Provide/inject example (would be used with child components)
provide('appName', 'Vue Demo App');
</script>

<template>
  <div class="max-w-4xl mx-auto p-6 font-sans text-gray-800">
    <!-- ===================
         First Component
         =================== -->

    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <h1 class="text-2xl font-bold text-blue-600 mb-4">{{ message }}</h1>
      <button
          @click="updateMessage"
          class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-md font-medium transition duration-200"
      >
        Update Message
      </button>

      <!-- Counter with manual controls instead of automatic increment -->
      <div class="mt-6 bg-white p-4 rounded-md shadow-sm">
        <div class="flex items-center justify-between">
          <h3 class="text-lg font-semibold text-gray-700">Counter:</h3>
          <div class="text-sm text-gray-500">Last updated: {{ lastUpdated }}</div>
        </div>

        <div class="flex items-center justify-center mt-4 gap-4">
          <button
              @click="decrementCount"
              class="bg-red-500 hover:bg-red-600 text-white w-10 h-10 rounded-full flex items-center justify-center text-xl font-bold transition duration-200"
          >
            -
          </button>

          <div class="text-3xl font-bold text-gray-800 w-16 text-center">
            {{ count }}
          </div>

          <button
              @click="incrementCount"
              class="bg-green-500 hover:bg-green-600 text-white w-10 h-10 rounded-full flex items-center justify-center text-xl font-bold transition duration-200"
          >
            +
          </button>
        </div>

        <button
            @click="resetCount"
            class="mt-4 w-full bg-gray-200 hover:bg-gray-300 text-gray-700 px-3 py-1 rounded-md font-medium text-sm transition duration-200"
        >
          Reset
        </button>
      </div>
    </section>

    <!-- ===================
         Second Component - v-if/v-else logic
         =================== -->

    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <div v-if="visible">
        <h1 class="text-red-500 text-xl font-bold">{{ helloworld }}</h1>
      </div>
      <div v-else>
        <h1 class="text-red-500 text-xl font-bold">Hello world from not visible</h1>
      </div>
    </section>

    <!-- ===================
         Status Toggle - if / else-if / else
         =================== -->

    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <button
          @click="toggleStatus"
          class="bg-gray-600 hover:bg-gray-700 text-white px-4 py-2 rounded-md font-medium transition duration-200"
      >
        Toggle Status
      </button>

      <p
          v-if="status === 'active'"
          class="mt-3 p-2 bg-green-100 text-green-800 rounded-md font-medium"
      >
        The status is active
      </p>
      <p
          v-else-if="status === 'inactive'"
          class="mt-3 p-2 bg-red-100 text-red-800 rounded-md font-medium"
      >
        The status is inactive
      </p>
      <p
          v-else
          class="mt-3 p-2 bg-yellow-100 text-yellow-800 rounded-md font-medium"
      >
        The status is unknown
      </p>
    </section>

    <!-- ===================
         NEW FEATURES FROM CHEAT SHEET
         =================== -->

    <hr class="my-8 border-gray-200">
    <h2 class="text-xl font-bold text-gray-700 mb-6 pb-2 border-b border-gray-200">Vue.js Directives & Event Binding Demo</h2>

    <!-- Form with v-model and event modifiers -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <form @submit.prevent="handleSubmit" class="max-w-md flex flex-col gap-4">
        <div class="flex flex-col gap-1">
          <label for="nameInput" class="font-medium text-gray-700">Name:</label>
          <input
              id="nameInput"
              v-model.trim="name"
              placeholder="Enter your name"
              class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
          >
        </div>
        <div class="flex flex-col gap-1">
          <label for="ageInput" class="font-medium text-gray-700">Age:</label>
          <input
              id="ageInput"
              v-model.number="user.age"
              type="number"
              class="px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
          >
        </div>
        <button
            type="submit"
            class="mt-2 w-full bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-md font-medium transition duration-200"
        >
          Submit Form
        </button>
      </form>
    </section>

    <!-- v-show demonstration -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <button
          @click="toggleVisibility"
          class="bg-purple-500 hover:bg-purple-600 text-white px-4 py-2 rounded-md font-medium transition duration-200"
      >
        Toggle Visibility
      </button>
      <p
          v-show="visible"
          class="mt-3 p-3 bg-gray-100 rounded-md"
      >
        This text uses v-show and will be hidden/shown
      </p>
    </section>

    <!-- Dynamic attribute binding -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <a
          :href="url"
          target="_blank"
          class="text-blue-500 hover:text-blue-600 hover:underline font-medium"
      >
        Visit Vue.js Website
      </a>
    </section>

    <!-- Admin toggle for v-if/v-else -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <button
          @click="toggleAdmin"
          class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-md font-medium transition duration-200"
      >
        Toggle Admin Status
      </button>
      <div class="mt-3">
        <p
            v-if="isAdmin"
            class="font-medium text-green-700"
        >
          Welcome Admin User
        </p>
        <p
            v-else
            class="text-gray-600"
        >
          Welcome Regular User
        </p>
      </div>
    </section>

    <!-- Dynamic class binding -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <div
          class="p-4 rounded-md mt-3 bg-gray-100 transition duration-300"
          :class="{
          'bg-blue-100 border border-blue-300': isActive,
          'border border-red-500': hasError
        }"
      >
        This div has dynamic classes
      </div>
    </section>

    <!-- Dynamic style binding -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <div
          :style="{ color: activeColor, fontWeight: 'bold' }"
          class="p-3 text-lg text-center"
      >
        This text has a dynamically bound color
      </div>
    </section>

    <!-- Event modifiers -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <button
          @click.once="loadOnce"
          class="bg-yellow-500 hover:bg-yellow-600 text-white px-4 py-2 rounded-md font-medium transition duration-200"
      >
        Click Once Demo
      </button>
      <div
          @mouseover="hoverFunc"
          class="mt-3 p-4 bg-gray-100 hover:bg-gray-200 rounded-md text-center cursor-pointer transition duration-200"
      >
        Hover over me
      </div>
    </section>

    <!-- v-for demonstration -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <div class="flex justify-between items-center mb-4">
        <h3 class="text-lg font-semibold text-gray-700">Todo Items ({{ activeItemsCount }} active)</h3>
        <button
            @click="addItem"
            class="bg-green-500 hover:bg-green-600 text-white px-3 py-1 rounded-md font-medium transition duration-200"
        >
          Add Item
        </button>
      </div>
      <ul class="space-y-2">
        <li
            v-for="item in items"
            :key="item.id"
            class="flex justify-between items-center p-3 bg-white rounded-md shadow-sm"
        >
          <span
              :class="{ 'line-through text-gray-400': item.completed }"
              class="font-medium"
          >
            {{ item.name }}
          </span>
          <div class="flex gap-2">
            <button
                @click="toggleItemCompletion(item.id)"
                class="px-3 py-1 text-sm rounded-md font-medium transition duration-200"
                :class="item.completed ? 'bg-blue-100 text-blue-700 hover:bg-blue-200' : 'bg-green-100 text-green-700 hover:bg-green-200'"
            >
              {{ item.completed ? 'Mark Active' : 'Mark Complete' }}
            </button>
            <button
                @click.stop="removeItem(item.id)"
                class="bg-red-100 text-red-700 hover:bg-red-200 px-3 py-1 text-sm rounded-md font-medium transition duration-200"
            >
              Remove
            </button>
          </div>
        </li>
      </ul>
    </section>


<!--  practice component section    -->
    <!-- New component section -->
    <section class="mb-8 bg-gray-50 p-6 rounded-lg shadow-sm">
      <h2 class="text-xl font-bold text-gray-700 mb-4">Practice Component</h2>
      <Practice />
    </section>
  </div>
</template>