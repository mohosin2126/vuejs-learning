<script setup>
import {ref} from "vue";
const helloworld = ref('');
helloworld.value = 'Hello World.';
const visiable = ref(true);
</script>

<template>
  <div v-if="visiable">
    <h1 class="text-red-500">{{helloworld}}</h1>
  </div>
  <div v-else>
    <h1 class="text-red-500">Hello world from not visible</h1>
  </div>
</template>

<style scoped>

</style>