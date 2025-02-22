<template>
  <task-component :assignment="tasks.TASK_3" />
  <task-component :assignment="tasks.TASK_6" />
</template>

<script>
import TaskComponent from './components/Task'
import { tasks } from './constants/assignments_text.js'

export default {
  name: 'App',
  components: {
    TaskComponent
  },
  data() {
    return {
      tasks: tasks
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
