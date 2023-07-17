<script>
import Header from "./components/Header.vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";

export default {
  name: 'App',
  components: {
    Header, TaskForm, TaskList
  },
  methods:
  {
    handleToggleShowForm() {
      this.showForm = !this.showForm
    },
    handleFormChangeEvent(e) {
      const { name, value, type, checked } = e.target
      if (type === 'checkbox') this.formValues = { ...this.formValues, [name]: checked }
      else this.formValues = { ...this.formValues, [name]: value }
    },
    handleSubmit(e) {
      e.preventDefault()
      const { description, date, time } =
        this.formValues
      if (description.length === 0 || date.length === 0 || time.length === 0) return
      this.tasks = [...this.tasks, { ...this.formValues, id: Math.random() }]
      this.formValues = {
        ...this.formValues, date: '',
        reminder: false,
        description: ''
      }
      e.target.reset()
    }
  },
  data: () => ({
    formValues: {
      date: '',
      time: '',
      reminder: false,
      description: ''
    },
    tasks: [],
    showForm: false
  }),
  created() {
    // this.tasks = [{}]
  },
  mounted() {
  },
  unmounted() { }
}
</script>
  
<template>
  <Header title='Task Tracker' @toggle-show-form='handleToggleShowForm' />
  <TaskList :tasks='tasks' />
  <TaskForm :show='showForm' @change-event='handleFormChangeEvent' @submit-event='handleSubmit' />
</template>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
