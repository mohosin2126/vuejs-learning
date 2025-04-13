<script setup>
import {ref} from "vue";

// <!-- state section -->

// Create a reactive reference variable named 'helloworld' initialized as an empty string
const helloworld = ref('');

// Set the value of 'helloworld' to 'Hello World.'
helloworld.value = 'Hello World.';

// Create a reactive boolean reference variable named 'visiable' initialized as true
const visiable = ref(true);



// <!-- If else part  -->
// Create a reactive status variable with initial value 'active'
const status = ref('active');

// Function to toggle between different status values
function toggleStatus() {
  // Check current status value
  if (status.value === 'active') {
    // If active, change to inactive
    status.value = 'inactive';
  } else if (status.value === 'inactive') {
    // If inactive, change to unknown
    status.value = 'unknown';
  } else {
    // For any other value, change back to active
    status.value = 'active';
  }
}
</script>

<template>
  <!-- Conditional rendering: this div will show if 'visiable' is true -->
  <div v-if="visiable">
    <!-- Heading with red text displaying the 'helloworld' value -->
    <h1 class="text-red-500">{{helloworld}}</h1>
  </div>
  <!-- Else condition: this div will show if 'visiable' is false -->
  <div v-else>
    <!-- Alternative heading with red text -->
    <h1 class="text-red-500">Hello world from not visible</h1>
  </div>



  // <!-- If else part   -->
  <div>
    <!-- Button that triggers the toggleStatus method when clicked -->
    <button @click="toggleStatus">Toggle Status</button>

    <!-- Paragraph shown only when status is 'active' -->
    <p v-if="status === 'active'">The status is active</p>
    <!-- Paragraph shown only when status is 'inactive' (if previous condition fails) -->
    <p v-else-if="status === 'inactive'">The status is inactive</p>
    <!-- Paragraph shown for any other status (if all previous conditions fail) -->
    <p v-else>The status is unknown</p>
  </div>
</template>

<style scoped>
/* Styles here would only apply to this component */
</style>