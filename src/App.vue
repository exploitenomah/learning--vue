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
    async addTask(newTask) {
      this.tasks = [...this.tasks, await this.createTask({ ...newTask, id: Math.random() })]
    },
    async removeTask(taskId) {
      const isDeleted = await this.deleteTask(taskId)
      if (isDeleted) this.tasks = [...this.tasks].filter(task => task.id !== taskId)
      else alert('Error deleting task')
    },
    async toggleReminder(taskId) {
      const taskToUpdate = await this.fetchTask(taskId)
      const updates = await this.updateTask({ ...taskToUpdate, reminder: !taskToUpdate.reminder })
      if (updates) this.tasks = updates
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
        if (res.status === 200) return await this.fetchTasks()
        else {
          const tasksInLsStr = localStorage.getItem('tasks')
          const tasksInLs = JSON.parse(tasksInLsStr)
          const modifiedTasksForLs = [...(tasksInLs || this.tasks).map(tsk => tsk.id === update.id ? update : tsk)]
          localStorage.setItem('tasks', JSON.stringify(modifiedTasksForLs))
          return modifiedTasksForLs
        }
      }
      catch (err) {
        console.error(err)
      }
    },
    async deleteTask(taskId) {
      try {
        const res = await fetch(`/api/tasks/${taskId}`, {
          method: 'DELETE'
        })
        if (res.status === 200) return res.status === 200
        else {
          const tasksInLsStr = localStorage.getItem('tasks')
          const tasksInLs = JSON.parse(tasksInLsStr)
          const modifiedTasksForLs = (tasksInLs || this.tasks).filter(tsk => tsk.id !== taskId)
          console.log(tasksInLs, modifiedTasksForLs)
          localStorage.setItem('tasks', JSON.stringify(modifiedTasksForLs))
          return true
        }
      } catch (err) {
        return false
      }
    }
  },
  data: () => ({
    tasks: [],
    showForm: false
  }),
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>
  
<template>
  <div class='h-screen overflow-hidden flex justify-center items-center'>
    <div
      class='h-screen relative overflow-auto w-[95vw] max-h-[90vh] max-w-[500px] mx-auto flex items-center flex-col border border/blue-600/30'>
      <Header :showForm='showForm' title='Task Tracker' @toggle-show-form='handleToggleShowForm' />
      <main className='w-full px-4 pb-5'>
        <TaskForm :show='showForm' @add-task='addTask' />
        <TaskList @toggle-reminder='toggleReminder' @delete-task='removeTask' :tasks='tasks' :show='tasks.length > 0' />
        <div v-show="tasks.length === 0">Tasks added will be shown here</div>
      </main>
    </div>
  </div>
</template>

