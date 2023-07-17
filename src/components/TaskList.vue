

<script>
import TaskItem from "./TaskItem.vue";
export default {
  name: "TaskListComponent",
  components: { TaskItem },
  props: {
    tasks: Array
  },
}

</script>

<template>
  <ul>
    <li v-for="task in tasks" :key="task.id">
      <TaskItem :task='task' />
    </li>
  </ul>
</template>

<style scoped></style>