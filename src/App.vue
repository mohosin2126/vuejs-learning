<script setup>
import {ref} from "vue";

// Create a reactive reference variable named 'helloworld' initialized as an empty string
const helloworld = ref('');

// Set the value of 'helloworld' to 'Hello World.'
helloworld.value = 'Hello World.';

// Create a reactive boolean reference variable named 'visiable' initialized as true
const visiable = ref(true);
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
</template>

<style scoped>
/* Styles here would only apply to this component */
</style>