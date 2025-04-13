---
title: Vue.js Directives & Event Binding Cheat Sheet
---

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
