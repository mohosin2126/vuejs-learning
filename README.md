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

# Vue.js Data Handling Guide

## Introduction

Vue.js offers elegant solutions for data fetching, state management, and rendering. This guide explores Vue's approach to handling data throughout the component lifecycle, with practical examples and best practices.

## Data Management in Vue

### Reactive State

Vue's Composition API provides `ref` and `reactive` for creating reactive state:

```js
import { ref, reactive } from 'vue'

// For primitive values
const count = ref(0)

// For objects
const user = reactive({
  name: 'Alice',
  email: 'alice@example.com'
})

// Accessing and mutating
console.log(count.value) // 0
count.value++

console.log(user.name) // 'Alice'
user.name = 'Bob'
```

### Computed Properties

Computed properties derive values from your state and automatically update:

```js
import { ref, computed } from 'vue'

const items = ref([1, 2, 3, 4, 5])

const evenItems = computed(() => {
  return items.value.filter(item => item % 2 === 0)
})

// evenItems.value is [2, 4]
// If items changes, evenItems updates automatically
```

## Data Fetching Patterns

### Basic Fetch on Mount

```js
import { ref, onMounted } from 'vue'

const posts = ref([])
const loading = ref(false)
const error = ref(null)

onMounted(async () => {
  loading.value = true
  try {
    const response = await fetch('https://api.example.com/posts')
    posts.value = await response.json()
  } catch (e) {
    error.value = e
  } finally {
    loading.value = false
  }
})
```

### Reusable Fetch Composable

```js
// useFetch.js
import { ref, isRef, unref, watchEffect } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)
  const loading = ref(false)

  function fetchData() {
    data.value = null
    error.value = null
    loading.value = true
    
    fetch(unref(url))
      .then(res => res.json())
      .then(json => {
        data.value = json
        loading.value = false
      })
      .catch(err => {
        error.value = err
        loading.value = false
      })
  }

  if (isRef(url)) {
    // If URL is reactive, re-fetch when it changes
    watchEffect(fetchData)
  } else {
    // Otherwise, just fetch once
    fetchData()
  }

  return { data, error, loading }
}

// Usage
const apiUrl = ref('https://api.example.com/posts')
const { data: posts, loading, error } = useFetch(apiUrl)

// Change URL to refetch automatically
apiUrl.value = 'https://api.example.com/posts?category=vue'
```

## Data Rendering in Templates

### List Rendering with v-for

```vue
<template>
  <div>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Error: {{ error.message }}</div>
    <ul v-else>
      <li v-for="post in posts" :key="post.id">
        <h3>{{ post.title }}</h3>
        <p>{{ post.body }}</p>
      </li>
    </ul>
  </div>
</template>
```

### Filtering and Sorting in Templates

```vue
<script setup>
import { ref, computed } from 'vue'

const searchQuery = ref('')
const todos = ref([
  { id: 1, text: 'Learn Vue', completed: false },
  { id: 2, text: 'Build a Vue app', completed: false },
  { id: 3, text: 'Share with community', completed: true }
])

const filteredTodos = computed(() => {
  return todos.value.filter(todo => 
    todo.text.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>

<template>
  <input v-model="searchQuery" placeholder="Search todos...">
  
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.completed">
      <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
    </li>
  </ul>
</template>

<style>
.completed {
  text-decoration: line-through;
  color: #999;
}
</style>
```

## State Management with Pinia

For larger applications, Pinia provides structured state management:

```js
// stores/courses.js
import { defineStore } from 'pinia'

export const useCourseStore = defineStore('courses', {
  state: () => ({
    courses: [],
    selectedCourses: [],
    loading: false,
    error: null
  }),
  
  getters: {
    totalCredits: (state) => {
      return state.selectedCourses.reduce((sum, course) => sum + course.creditInHours, 0)
    },
    remainingCredits: (state) => {
      const maxCredits = 18
      return maxCredits - state.totalCredits
    }
  },
  
  actions: {
    async fetchCourses() {
      this.loading = true
      try {
        const res = await fetch('/api/courses')
        this.courses = await res.json()
      } catch (error) {
        this.error = error
      } finally {
        this.loading = false
      }
    },
    
    selectCourse(course) {
      if (this.selectedCourses.some(c => c.id === course.id)) {
        return false
      }
      this.selectedCourses.push(course)
      return true
    }
  }
})

// Usage in component
import { useCourseStore } from '@/stores/courses'

const courseStore = useCourseStore()

// On component mount
onMounted(() => {
  courseStore.fetchCourses()
})
```

## Working with Forms

```vue
<script setup>
import { ref } from 'vue'

const formData = ref({
  name: '',
  email: '',
  message: ''
})

const errors = ref({})
const submitted = ref(false)

const validateForm = () => {
  errors.value = {}
  
  if (!formData.value.name) {
    errors.value.name = 'Name is required'
  }
  
  if (!formData.value.email) {
    errors.value.email = 'Email is required'
  } else if (!/^\S+@\S+\.\S+$/.test(formData.value.email)) {
    errors.value.email = 'Email is invalid'
  }
  
  if (!formData.value.message) {
    errors.value.message = 'Message is required'
  }
  
  return Object.keys(errors.value).length === 0
}

const submitForm = async () => {
  if (validateForm()) {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000))
    submitted.value = true
    formData.value = { name: '', email: '', message: '' }
  }
}
</script>

<template>
  <div>
    <div v-if="submitted" class="success-message">
      Thank you for your submission!
    </div>
    
    <form @submit.prevent="submitForm" v-else>
      <div class="form-group">
        <label for="name">Name</label>
        <input 
          id="name" 
          v-model="formData.name" 
          :class="{ 'error': errors.name }"
        >
        <p v-if="errors.name" class="error-text">{{ errors.name }}</p>
      </div>
      
      <div class="form-group">
        <label for="email">Email</label>
        <input 
          id="email" 
          type="email" 
          v-model="formData.email"
          :class="{ 'error': errors.email }"
        >
        <p v-if="errors.email" class="error-text">{{ errors.email }}</p>
      </div>
      
      <div class="form-group">
        <label for="message">Message</label>
        <textarea 
          id="message" 
          v-model="formData.message"
          :class="{ 'error': errors.message }"
        ></textarea>
        <p v-if="errors.message" class="error-text">{{ errors.message }}</p>
      </div>
      
      <button type="submit">Submit</button>
    </form>
  </div>
</template>
```

## Router Configuration

The application uses Vue Router with memory history mode. This is set up in `routes/index.js`:

```js
import { createMemoryHistory, createRouter } from 'vue-router'
import Home from "@/home/index.vue";

const routes = [
    { path: '/', component: Home },
]

const router = createRouter({
    history: createMemoryHistory(),
    routes,
})

export default router;
```

Memory history mode is useful for environments where you don't have access to the browser history API, such as in some server environments or embedded applications.

## Root App Component

The `App.vue` component serves as the application shell and contains the router view:

```vue
<script>
// Component logic goes here
</script>

<template>
  <router-view/>
</template>
```

The `<router-view/>` is a special component that renders the matched component for the current route.

## Application Entry Point

The `main.js` file serves as the entry point for the application:

```js
import './style.css'
import { createApp } from 'vue'
import App from './App.vue'
import router from "@/routes/index.js";

createApp(App)
    .use(router)
    .mount('#app')
```



## Best Practices

1. **Separate concerns** using composables for reusable logic
2. **Prefer Composition API** for complex components and better TypeScript support
3. **Use computed properties** instead of methods for derived state
4. **Keep components focused** on a single responsibility
5. **Handle loading and error states** explicitly in your UI
6. **Use Pinia** for global state management instead of passing props deeply
7. **Leverage Vue DevTools** for debugging and inspecting your app's state




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
