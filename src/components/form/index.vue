<script setup>
import {ref} from "vue";

const form=ref({
  name:'',
  email:'',
  password:'',
  gender:'',
  phone:'',
})
console.log(form);

</script>

<template>
  <div >
    <h1 class="font-bold text-center mt-4 ">
      Practice Form
    </h1>
{{form}}
<form @submit.prevent="submit" class="space-y-4">
<input type="text" v-model="form.name" placeholder="Name" required />
<div>
  <label for="gender">Gender</label>
  <select v-model="form.gender">
    <option value="male">Male</option>
    <option value="female">Female</option>
  </select>
</div>
  <button type="submit" class="mt-4 block bg-red-400 border-2 p-2 rounded-2xl">Submit</button>
</form>

  </div>
</template>