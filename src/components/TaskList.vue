

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
      <TaskItem @toggle-my-reminder='$emit("toggle-reminder", task.id)' @delete-me='$emit("delete-task", task.id)'
        :task='task' />
    </li>
  </ul>
</template>

<style scoped></style>