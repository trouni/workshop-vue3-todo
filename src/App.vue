<script setup>
import Github from '@/components/icons/GitHub.vue';
import Linkedin from '@/components/icons/LinkedIn.vue';
import TasksList from './components/TasksList.vue';
</script>

<template>
  <header>
    <img alt="Vue logo" src="./assets/logo.svg" height="100" />
    <h1>Task Manager</h1>
  </header>

  <main>
    <TasksList />
  </main>

  <footer>
    <p>Workshop by Trouni Tiet</p>
    <div class="logos">
      <a href="https://www.linkedin.com/in/trouni" target="_blank">
        <Linkedin />
      </a>
      <a href="https://www.github.com/trouni" target="_blank">
        <Github />
      </a>
    </div>
  </footer>
</template>


<style lang="scss" scoped>
header {
  text-align: center;
  color: #2c3e50;
  margin: auto;
  margin-top: 60px;
  min-height: 20vh;
}

main {
  text-align: center;
  color: #2c3e50;
  margin: auto;
  padding: 3em 0 3em 0;
  max-width: 600px;
  min-height: 50vh;
}

footer {
  text-align: center;
  margin: 2em 0;
  font-size: 0.8em;
  color: #aaa;
}

.logos svg {
  margin: 0.8em;
  fill: #aaa;

  &:hover {
    fill: #2c3e50;
  }
}
</style>
