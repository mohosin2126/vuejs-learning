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
