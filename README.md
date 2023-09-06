### JavaScript Workshop
# Introduction to Vue.js

<p align="center" width="100%">
  <img src="src/assets/logo.svg" alt="Vue.js Logo" height="100px" style="margin: auto;">
</p>

Learn how to build <a href="https://trouni-vue-task-manager.netlify.app/" target="_blank">this simple Todo app</a>.

Note: the slide version of this workshop is available [here](https://slides.trouni.com/?src=trouni/workshop-vue3-todo).

---

## Setup

**TODO:**

Clone the git repository for this workshop and get into the project folder.

```sh
gh repo clone trouni/workshop-vue3-todo
# OR if you don't have the gh CLI:
# git clone git@github.com:trouni/workshop-vue3-todo.git

cd workshop-vue3-todo
```

**OR** [download the ZIP file](https://github.com/trouni/workshop-vue3-todo/archive/refs/heads/main.zip) and unzip the archive to your projects folder or desktop.


**TODO:**

Run `yarn install` to install the project's dependencies, then `yarn dev` to launch a local server.

```sh    
yarn install # Installs dependencies

yarn dev # Compiles and hot-reloads for development
```

You should now be able to view your app on http://localhost:3000.


### Optional Setup

Not required, but you can also install these useful tools:
- [Vue Language Features (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.volar), a language support extension built for Vue.
- [Vue.js devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd), a Chrome extension to debug your Vue app from the browser.

---

## Intro to Vue Components


### Component File Structure

```html
<!-- Component.vue -->

<script>
  // Data & logic
</script>

<template>
  <!-- HTML structure -->
</template>

<style>
  /* Styling */
</style>
```


### Creating your first components

Create a new `/components/TasksList.vue` file with the following code:

```html
<!-- TasksList.vue -->

<template>
  <p>This is my first Vue.js component</p>
</template>
```


Then import the component in your `App.vue` file.

```vue
<!-- App.vue -->

<script setup>
import TasksList from './components/TasksList.vue';
</script>

<template>
  <!-- ... -->

  <main>
    <img alt="Vue logo" src="./assets/logo.svg" height="100" />
    <h1>Task Manager</h1>
    <TasksList />
  </main>

  <!-- ... -->
</template>
```


Let's replace the text with a static card:

```html
<!-- TasksList.vue -->

<template>
  <div class="task-card">
    <div>
      <h3>Create a card component</h3>
      <p>Create a new TaskCard.vue file in the components folder, then import it in TasksList.vue</p>
    </div>
  </div>
</template>
```


### Styling components

You can add styling rules for each component in the `<style>` section. Try it out!


You can use this styling I prepared for the card, but we should first move the task card into its own component. Move the previous code into a `TaskCard.vue` and add this styling:

```vue
<!-- TaskCard.vue -->

<style lang="scss">
.task-card {
  display: flex;
  min-height: 6rem;
  text-align: left;
  background-color: saturate(rgba(#41b883, 0.03), 30%);
  border-bottom: solid 1px rgba(#35495e, 0.1);
  border-left: solid 8px #41b883;
  transition: all 0.3s;
  p { font-size: 0.9rem; }
  &:hover {
    transform: scale(1.02);
    background-color: white;
    box-shadow: 2px 3px 10px rgba(black, 0.1);
  }
  & > div:first-child {
    margin: 0 1rem;
    padding: 1rem 0;
    display: flex;
    flex-grow: 1;
    flex-direction: column;
    justify-content: center;
  }
  &.done {
    border-left: solid 8px rgba(#35495e, 0.3);
    background-color: rgba(#35495e, 0.08);
    h3, p { text-decoration: line-through; opacity: 0.3; }
    &:hover { transform: unset; box-shadow: unset; }
  }
}
</style>
```

---

## Making dynamic components


### Defining the State of a Component

Any top-level bindings (including variables, function declarations, and imports) declared inside `<script setup>` are directly usable in the template using the `{{ variable }}` syntax:

```vue
<!-- TaskCard.vue -->

<script setup>
// Any variable or function defined here is directly accessible in the template
const title = "Make the card component dynamic"
const description = "Learn about using the data option and passing data to child components using props"
const done = false
</script>

<template>
  <div class="task-card">
    <div>
      <h3>{{ title }}</h3>
      <p>{{ description }}</p>
    </div>
    <div>{{ done ? "✅" : "⭕️" }}</div>
  </div>
</template>
```


### Make the data reactive using `ref`

We need to tell Vue that the component should be updated every time a change is detected on these variables. We make them reactive using `ref`.

```vue
<!-- TaskCard.vue -->

<script setup>
import { ref } from 'vue';

const title = ref("Make the card component dynamic")
const description = ref("Learn about using the data option and passing data to child components using props")
const done = ref(false)
</script>
```

**💡Tip**: Use the [Vue.js devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd), to interact with your data and see how changes re-render the component.


**We want our component to be reusable:**

Let's assign the `title` and `description` dynamically.


### Passing Data to Child Components with `props`

> Props are custom attributes you can register on a component. When a value is passed to a prop attribute, it becomes a property on that component instance. 

Props are basically special data attributes **that come from the parent component**.


Let's replace our variables and define some props using `defineProps`:

```vue
<!-- TaskCard.vue -->

<script setup>
defineProps({
  title: String,
  description: String,
  done: { type: Boolean, default: false }
})
</script>
```


We can now pass the `title` and `description` when rendering the `TaskCard` component instead (in the parent component):

```vue
<!-- TasksList.vue <template> -->

<TaskCard
  title="Make the card component dynamic"
  description="Learn about using the data option and passing data to child components using props"
  done="true"
/>
```

---

## Vue Directives


Let's define multiple tasks to make things more interesting! Instead of `ref`, we use `reactive` to make objects reactive.

```vue
<!-- TasksList.vue -->

<script setup>
import { reactive } from 'vue';

const tasks = reactive([
  {
    title: "Create a card component",
    description:
      "Create a new TaskCard.vue file in the components folder, then import it in TasksList",
    done: true,
  },
  {
    title: "Make the card component dynamic",
    description:
      "Learn about using the data option and passing data to child components using props",
    done: true,
  },
  {
    title: "Bind the attributes to the data",
    description:
      "Use the v-bind directive to bind the title and description to our data",
    done: false,
  },
])
</script>
```


### Binding Attributes to Props and Data with `v-bind`

We want to use the values from our `tasks` in the `title` and `description` attributes of our `TaskCard` component, but mustaches cannot be used inside HTML attributes.

Instead, we use a `v-bind` directive (shorthand `:`).

```vue
<!-- TasksList.vue -->

<template>
  <TaskCard v-bind:title="tasks[0].title" v-bind:description="tasks[0].description" v-bind:done="tasks[0].done" />
  <!-- OR using the shorthand: -->
  <TaskCard :title="tasks[1].title" :description="tasks[1].description" :done="tasks[1].done" />
</template>
```


### Binding HTML Classes

We can apply classes conditionally too!. For example, we add a `.done` class to the card when the task is completed (`done` is `true`).

```vue
<!-- TaskCard.vue -->
<template>
  <!-- <div :class="['my-first-class', 'another-class', { 'a-conditional-class': conditionToAddTheClass }]"> -->
  <div :class="['task-card', { done: done }]">
</template>
```


### Iterate with `v-for`

Let's now display all of our tasks by iterating over the `tasks` array using the `v-for` directive.

```vue
<!-- TasksList.vue <template> -->

<TaskCard
  v-for="(task, index) in tasks"
  :key="index"
  :title="task.title"
  :description="task.description"
  :done="task.done"
/>
```


### Conditional Rendering with `v-if` / `v-else`

Let's display a small message when we have no tasks in our app.

```vue
<!-- TasksList.vue <template> -->

<template>
  <div v-if="tasks.length > 0" class="tasks-list">
    <!-- TaskCard -->
  </div>
</template>
<p v-else>You don't have any tasks yet...</p>
```


### Adding Behavior with `methods`

Implement an `addTask()` function to push a new task at the beginning of the `tasks` array.

```vue
<!-- TasksList.vue -->

<script setup>
// ...
function addTask(title, description, done = false) {
  tasks.unshift({ title, description, done });
}
// ...
</script>
```


### Listen for events with `v-on`

Let's add a button to test that our method works and make it listen to `click` events using `v-on` (shorthand `@`).

```vue
<!-- TasksList.vue <template> -->

<template>
  <button class="btn round-icon" v-on:click="addTask('My new task', 'My new description')">＋</button>
</template>
```


### Capturing user input with `v-model`

We still need to be able to enter the `title` and `description` ourselves. Let's add some inputs to our `TasksList`.

```vue
<!-- TasksList.vue <template> -->
<template>
  <div class="task-card new-task">
    <div>
      <input type="text" placeholder="What would you like to do?" />
      <textarea placeholder="Add some details about your task..."></textarea>
    </div>
  </div>
</template>
```


Add the following styles to your `TasksList` component:

```vue
<!-- TasksList.vue -->

<style lang="scss" scoped>
@keyframes expand-vertical { from { min-height: 0; height: 0; } to { min-height: 6rem; } }
.task-card.new-task {
  animation: expand-vertical 0.2s;
  overflow: hidden;
  background-color: white;
  border-left: solid 5px #35495e;
  &, &:hover { transform: scale(1.1); box-shadow: 2px 3px 10px rgba(black, 0.2); }
  & + .tasks-list { pointer-events: none; }
  input { font-size: 1.17rem; font-weight: bold; width: 100%; }
  textarea { width: 100%; font-size: 0.9rem; resize: none; }
}
</style>
```


We can bind the input fields with data attributes in our component.

```vue
<!-- TasksList.vue -->

<script setup>
// ...
const newTitle = ref('')
const newDescription = ref('')
</script>

<template>
  <!-- ... -->
    <input type="text" placeholder="What would you like to do?"
      v-model="newTitle"
    />
    <textarea placeholder="Add some details about your task..."
      v-model="newDescription"
    ></textarea>
  <!-- ... -->
</template>
```


### Update the `v-on` directive

You can chain actions by separating them with a comma:

```vue
<!-- TasksList.vue -->

<template>
  <!-- ... -->
  <button
    class="btn round-icon"
    @click="addTask(newTitle, newDescription), resetForm()"
  >＋</button>
  <!-- ... -->
</template>
```


Define the new `resetForm()` function. Note that reactive properties need to be updated using the `.value` attribute.

```vue
<!-- TasksList.vue -->

<script setup>
// ...
function addTask(title, description, done = false) {
  tasks.unshift({ title, description, done });
}

function resetForm() {
  newTitle.value = ""
  newDescription.value = ""
}
// ...
</script>
```


### Manipulate the DOM by combining `v-on` and `v-if`/`v-show`

Let's only show the inputs after we click on the `+` button.

```vue
<!-- TasksList.vue -->

<script setup>
// ...
const newFormVisible = ref(false)

// ...
function resetForm() {
  newTitle.value = ""
  newDescription.value = ""
  newFormVisible.value = false;
}
</script>

<template>
  <div>
    <button class="btn round-icon" @click="newFormVisible = !newFormVisible">
      {{ newFormVisible ? "✕" : "＋" }}
    </button>

    <div
      v-show="newFormVisible"
      class="task-card new-task"
      @keyup.enter="addTask(newTitle, newDescription), resetForm()"
    >
    <!-- ... -->
    <p v-else-if="!newFormVisible">You don't have any tasks yet...</p>
  </div>
</template>
```

---

## Bonus


### Persist Data with `localStorage`

We can easily store the `tasks` array directly in the user's browser:

```vue
<!-- TasksList.vue -->

<script setup>
import { reactive, ref, watch } from 'vue';
// ...
const tasks = reactive(JSON.parse(localStorage.getItem("tasks")) || [])

watch(tasks, (newX) => {
  localStorage.setItem("tasks", JSON.stringify(tasks));
})

function clearTasks() {
  tasks.splice(0)
}
// ...
</script>
```


### Send Data to Parent Components using Custom Events

To pass data **upstream** (_i.e._ from a child to a parent component): 
1. the child component emits a custom event using `$emit('custom-event')`
2. the parent component listens for that event using `v-on:custom-event=` (or `@custom-event=`)


Let's move the input fields and style from `TasksList` to a new component `NewTask`. In this component, we remove the `addTask` method and replace it with `$emit` to send a custom event upstream, that contains the new title and description:

```vue
<!-- NewTask.vue -->

<script setup>
import { ref } from 'vue';

const newTitle = ref('')
const newDescription = ref('')

function resetForm() {
  newTitle.value = ''
  newDescription.value = ''
}
</script>

<template>
  <div class="task-card new-task" @keyup.enter="$emit('add-task', newTitle, newDescription), resetForm()">
    <!-- ... -->
  </div>
</template>

<style lang="scss" scoped>
// Don't forget to copy over the styling
</style>
```


The parent component `TasksList` listens for the custom `add-task` event, and calls the `addTask` method when the event is triggered.

```vue
<!-- TasksList.vue -->

<template>
  <!-- ... -->
  <NewTask @add-task="addTask" />
  <!-- ... -->
</template>
```


### Mark Tasks as Done

Implement this  `toggleTask` method in the `TasksList` component:

```vue
<!-- TasksList.vue -->

<script setup>
// ...
function toggleTask(taskIndex) {
  const taskToUpdate = tasks[taskIndex];
  taskToUpdate.done = !taskToUpdate.done;
}
</script>
```


Create this new UI component `Checkbox`:

```vue
<!-- Checkbox.vue -->

<script setup>
defineProps({
  checked: Boolean
})
</script>

<template>
  <div :class="['checkbox', { checked }]"></div>
</template>

<style lang="scss" scoped>
$check-color: #41b883;
.checkbox {
  display: flex;
  align-self: center;
  border: solid 2px $check-color;
  border-radius: 50%;
  width: 2rem;
  height: 2rem;
  margin: 0 1rem;
  cursor: pointer;
  &:hover { background-color: rgba($check-color, 0.2); transform: scale(1.1); }
  &.checked { background-color: $check-color; }
  &.checked:after {
    content: "";
    display: inline-block;
    transform: rotate(45deg) translate(-35%, 40%);
    height: 1rem;
    width: 1rem;
    margin-left: 60%;
    border-bottom: 5px solid white;
    border-right: 5px solid white;
  }
}
</style>
```


Modify your `TaskCard` component to use the `Checkbox` and `$emit` a custom event on `click`.

```vue
<!-- TaskCard.vue -->

<script setup>
import Checkbox from "./Checkbox";

defineProps({
  title: String,
  description: String,
  done: { type: Boolean, default: false },
  taskIndex: Number, // <-- Add this as well
})
</script>

<template>
  <div :class="['task-card', { done }]">
    <div>
      <h3>{{ title }}</h3>
      <p>{{ description }}</p>
    </div>
    <Checkbox @click="$emit('toggle-task', taskIndex)" :checked="done" />
  </div>
</template>
```


Finally, update your `TaskCard` component in your `TasksList.vue` file to pass the task index and listen for the `toggle-task` event:

```vue
<!-- TasksList.vue -->

<template>
  <!-- ... -->
  <TaskCard v-for="(task, index) in tasks" :key="index" :title="task.title" :description="task.description" :done="task.done"
            :task-index="index"
            @toggle-task="toggleTask"
  />
  <!-- ... -->
</template>
```

---

## The Finished App

You can view the finished code for the app [here](https://github.com/trouni/workshop-vue3-todo/tree/solution).

---

## What to look at next?

- [Computed Properties](https://vuejs.org/api/reactivity-core.html#computed)
- [Vue Router](https://router.vuejs.org/)
- [Component Slots](https://vuejs.org/guide/components/slots.html)
- [Vue Lifecycle Hooks](https://vuejs.org/api/composition-api-lifecycle.html)

---

## Happy coding!

Workshop by **Trouni Tiet**\
[LinkedIn](https://linkedin.com/trouni) | [GitHub](https://github.com/trouni)
