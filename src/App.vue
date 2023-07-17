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
    resetFormValues() {
      this.formValues = {
        ...this.formValues, date: '',
        reminder: false,
        time: '',
        description: ''
      }
    },
    async addTask(newTask) {
      this.tasks = [...this.tasks, await this.createTask({ ...newTask, id: Math.random() })]
      this.resetFormValues()
    },
    async removeTask(taskId) {
      const isDeleted = await this.deleteTask(taskId)
      if (isDeleted) this.tasks = this.tasks.filter(task => task._id === taskId)
      else alert('Error deleting task')
    },
    async toggleReminder(taskId) {
      const taskToUpdate = await this.fetchTask(taskId)
      const update = await this.updateTask({ ...taskToUpdate, reminder: !taskToUpdate.reminder })
      if (update) this.tasks = this.tasks.map(task => task._id === update.id ? update : task)
      else alert('Error updating task')
    },
    async fetchTasks() {
      try {
        const res = await fetch('/api/tasks')
        const tasks = await res.json()
        if (res.status === 200) return tasks
      } catch (err) {
        const tasksInLsStr = localStorage.getItem('tasks')
        const tasksInLs = JSON.parse(tasksInLsStr)
        return Array.isArray(tasksInLs) ? tasksInLs : []
      }

    },
    async fetchTask(taskId) {
      try {
        const res = await fetch(`/api/tasks/${taskId}`)
        const task = await res.json()
        return task
      }
      catch (err) {
        const tasksInLsStr = localStorage.getItem('tasks')
        const tasksInLs = JSON.parse(tasksInLsStr)
        return (tasksInLs || []).find(tsk => tsk.id === taskId)
      }
    },
    async createTask(task) {
      try {
        const res = await fetch('/api/tasks', {
          method: 'POST',
          body: JSON.stringify(task),
          headers: {
            'Content-type': 'application/json',
          }
        })
        return await res.json()
      }
      catch (err) {
        const tasksInLsStr = localStorage.getItem('tasks')
        const tasksInLs = JSON.parse(tasksInLsStr)
        const modifiedTasksForLs = [...(tasksInLs || []), task]
        localStorage.setItem('tasks', JSON.stringify(modifiedTasksForLs))
        return task
      }
    },
    async updateTask(update) {
      try {
        const res = await fetch(`/api/tasks/${update.id}`, {
          method: 'PUT',
          body: JSON.stringify(update),
          headers: {
            'Content-type': 'application/json',
          }
        })
        return await res.json()
      }
      catch (err) {
        const tasksInLsStr = localStorage.getItem('tasks')
        const tasksInLs = JSON.parse(tasksInLsStr)
        const modifiedTasksForLs = [...(tasksInLs || []).map(tsk => tsk.id === update.id ? update : tsk)]
        localStorage.setItem('tasks', JSON.stringify(modifiedTasksForLs))
        return update
      }
    },
    async deleteTask(taskId) {
      const res = await fetch(`/api/tasks/${taskId}`, {
        method: 'DELETE'
      })
      if (res.status === 200) return res.status === 200
      else {
        const tasksInLsStr = localStorage.getItem('tasks')
        const tasksInLs = JSON.parse(tasksInLsStr)
        const modifiedTasksForLs = (tasksInLs || []).filter(tsk => tsk.id === taskId)
        localStorage.setItem('tasks', JSON.stringify(modifiedTasksForLs))
        return true
      }
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
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>
  
<template>
  <Header title='Task Tracker' @toggle-show-form='handleToggleShowForm' />
  <TaskList @toggle-reminder='toggleReminder' @delete-task='removeTask' :tasks='tasks' :show='tasks.length > 0' />
  <div v-show="tasks.length === 0">Tasks added will be shown here</div>
  <TaskForm :show='showForm' @change-event='handleFormChangeEvent' @add-task='addTask' />
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
