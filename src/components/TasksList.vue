<script setup>
import { reactive, ref, watch } from 'vue';
import TaskCard from './TaskCard.vue';
import NewTask from './NewTask.vue';

// ===== Tasks list ===== //

const tasks = reactive(JSON.parse(localStorage.getItem("tasks")) || [])

watch(tasks, (newTasks) => {
  localStorage.setItem("tasks", JSON.stringify(newTasks));
})

function clearTasks() {
  tasks.splice(0)
}

// ===== Creating a task ===== //

const newFormVisible = ref(tasks.length == 0);

function addTask(title, description, done = false) {
  tasks.unshift({ title, description, done });
  newFormVisible.value = false
}

// ===== Toggling a task ===== //

function toggleTask(taskIndex) {
  const taskToUpdate = tasks[taskIndex];
  taskToUpdate.done = !taskToUpdate.done;
}
</script>

<template>
  <button class="btn round-icon" @click="newFormVisible = !newFormVisible">
    {{ newFormVisible ? "✕" : "＋" }}
  </button>
  <NewTask v-show="newFormVisible" @add-task="addTask" />
  <div v-if="tasks.length > 0">
    <TaskCard v-for="(task, index) in tasks" :key="index" :title="task.title" :description="task.description"
      :done="task.done" :task-index="index" @toggle-task="toggleTask" />
    <p class="clear-tasks" @click="clearTasks">Clear tasks</p>
  </div>
  <p v-else-if="!newFormVisible">You don't have any tasks yet...</p>
</template>

<style lang="scss" scoped>
.clear-tasks {
  font-size: 0.8em;
  cursor: pointer;
  margin: 2em;
  color: #aaa;

  &:hover {
    color: #41b883;
  }
}
</style>