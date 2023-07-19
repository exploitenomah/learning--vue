

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
  <div>
    <h2 className='text-center text-2xl capitalize mt-4 mb-2'>Tasks</h2>
    <ul className='gap-y-4 flex flex-col'>
      <li v-for="task in tasks" :key="task.id">
        <TaskItem @toggle-my-reminder='$emit("toggle-reminder", task.id)' @delete-me='$emit("delete-task", task.id)'
          :task='task' />
      </li>
    </ul>
  </div>
</template>

<style scoped></style>