# Vue.js Composition API: Reactive State, Conditional Rendering, and Lifecycle Hooks

## Reactive State in Vue Composition API

The Composition API offers two main ways to create reactive state:

- `ref()`: For primitive values (strings, numbers, booleans)
- `reactive()`: For objects and arrays

```javascript
import { ref, reactive } from 'vue'

// Using ref for primitive values
const count = ref(0)      // Access/modify with .value
const message = ref('')   // Access/modify with .value

// Using reactive for objects
const user = reactive({
  name: 'John',
  age: 30,
  preferences: {
    theme: 'dark'
  }
})  // Access/modify directly (user.name, user.preferences.theme)
```

## Conditional Rendering

Vue provides directives to conditionally render elements:

### v-if / v-else-if / v-else

Completely adds or removes elements from the DOM:

```html
<div v-if="status === 'active'">Active Status</div>
<div v-else-if="status === 'inactive'">Inactive Status</div>
<div v-else>Unknown Status</div>
```

### v-show

Toggles the CSS `display` property (element remains in DOM):

```html
<div v-show="isVisible">This content toggles visibility</div>
```

## Component Lifecycle Hooks

The Composition API provides lifecycle hooks as functions:

```javascript
import {
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted
} from 'vue'

// Setup is called when the component is first initialized
// (similar to the created hook in Options API)

onBeforeMount(() => {
  // Called right before the component is about to mount
  // DOM isn't available yet
})

onMounted(() => {
  // Called after the component has been mounted
  // Good place for DOM interactions, API calls, etc.
})

onBeforeUpdate(() => {
  // Called when data changes, before the DOM updates
})

onUpdated(() => {
  // Called after data changes have caused the component to re-render
})

onBeforeUnmount(() => {
  // Called right before the component is unmounted
  // Good place for cleanup
})

onUnmounted(() => {
  // Called after the component has been unmounted
  // Component is completely destroyed
})
```

## Complete Example

Here's a complete example demonstrating reactive state, conditional rendering, and lifecycle hooks:

```vue
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
```

## Additional Features

### Watchers

```javascript
import { ref, watch, watchEffect } from 'vue'

const searchQuery = ref('')
const results = ref([])

// Watch specific value
watch(searchQuery, async (newValue, oldValue) => {
  if (newValue.trim()) {
    results.value = await fetchSearchResults(newValue)
  } else {
    results.value = []
  }
})

// watchEffect runs immediately and tracks dependencies automatically
watchEffect(() => {
  console.log(`Current query: ${searchQuery.value}`)
})
```

### Computed Properties

```javascript
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

// Read-only computed property
const fullName = computed(() => {
  return `${firstName.value} ${lastName.value}`
})
```

### Composables (Reusable Logic)

```javascript
// useCounter.js
import { ref } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)
  
  function increment() {
    count.value++
  }
  
  function decrement() {
    count.value--
  }
  
  return { count, increment, decrement }
}

// In component:
import { useCounter } from './useCounter'

// In setup()
const { count, increment, decrement } = useCounter(10)
```

### Lifecycle Hooks with Components

```javascript
// Parent component
const ParentComponent = {
  setup() {
    onMounted(() => {
      console.log('Parent component mounted')
    })
  }
}

// Child component
const ChildComponent = {
  setup() {
    onMounted(() => {
      console.log('Child component mounted')
    })
  }
}

// Output when parent with child is mounted:
// 'Child component mounted'
// 'Parent component mounted'
```


# 🔗 Vue.js Directives & Event Binding

| **Feature**              | **Syntax**              | **Example**                                                   |
|--------------------------|-------------------------|----------------------------------------------------------------|
| Click event              | `@click`                | `<button @click="doSomething">Click Me</button>`               |
| Mouseover event          | `@mouseover`            | `<div @mouseover="hoverFunc">Hover here</div>`                |
| Input binding            | `v-model`               | `<input v-model="name" />`                                     |
| Show/Hide element        | `v-show`                | `<p v-show="isVisible">Visible Text</p>`                        |
| Conditional render       | `v-if / v-else-if / v-else` | `<p v-if="isAdmin">Admin</p><p v-else>User</p>`          |
| Looping                  | `v-for`                 | `<li v-for="item in items" :key="item.id">{{ item.name }}</li>`|
| Binding attribute        | `:href / v-bind:href`   | `<a :href="url">Visit</a>`                                     |
| Dynamic class            | `:class`                | `<div :class="{ active: isActive }"></div>`                    |
| Dynamic style            | `:style`                | `<div :style="{ color: activeColor }"></div>`                  |
| Submit with prevent      | `@submit.prevent`       | `<form @submit.prevent="handleSubmit">`                        |
| Stop event bubbling      | `@click.stop`           | `<button @click.stop="handleClick">Click</button>`             |
| Trigger once             | `@click.once`           | `<button @click.once="loadOnce">Load Once</button>`            |
| Lazy input update        | `v-model.lazy`          | `<input v-model.lazy="msg" />`                                 |
| Trim input               | `v-model.trim`          | `<input v-model.trim="name" />`                                |
| Convert to number        | `v-model.number`        | `<input v-model.number="age" />`                               |


# 📦 Vue.js Components & Props

| **Feature**               | **Syntax**                          | **Example**                                                                 |
|---------------------------|-------------------------------------|------------------------------------------------------------------------------|
| Register component        | `app.component('MyComp', {...})`    | `app.component('MyBtn', { template: '<button>Click</button>' })`            |
| Use component             | `<ComponentName />`                 | `<MyBtn />`                                                                 |
| Props in child            | `props: ['title']`                  | `props: ['label']` inside child                                             |
| Pass props from parent    | `:propName="value"`                 | `<Child :title="parentTitle" />`                                           |
| Emit event from child     | `this.$emit('eventName')`           | `this.$emit('submit')` inside child                                        |
| Listen to event in parent | `@eventName`                        | `<Child @submit="handleSubmit" />`                                         |
| Slot (default)            | `<slot></slot>`                     | `<Child><p>Content</p></Child>`                                            |
| Named slot                | `<slot name="header"></slot>`       | `<template v-slot:header><h1>Hi</h1></template>`                            |
| Scoped slot               | `<slot :data="value" />`            | `<template v-slot="{ data }">{{ data }}</template>`                         |
