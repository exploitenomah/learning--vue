
<script>
import Button from './Button.vue'
import ToggleComponent from './Toggler.vue'
export default {
  name: 'TaskFormComponent',
  components: {
    Button, ToggleComponent
  },
  props: {
    show: Boolean,
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault()
      const { description, date, time, reminder } = e.target.elements
      if (description.value.length === 0 || date.value.length === 0 || time.value.length === 0) return alert('Please add aa task')
      const newTask = {
        date: date.value, time: time.value,
        reminder: reminder.checked, description: description.value,
      }
      this.$emit('add-task', newTask)
      this.date = '',
        this.time = '',
        this.reminder = false,
        this.description = ''
      e.target.reset()
    }
  },
  data() {
    return {
      date: '',
      time: '',
      reminder: false,
      description: ''
    }
  }
}

</script>

<template>
  <form
    :class='[show ? "max-h-[100vh]" : "max-h-[0]", "overflow-hidden transition-[max-height] duration-[400ms] ease-in-out"]'
    v-on:submit="handleSubmit">
    <label>
      <span>Task Description</span><input type='text' required v-model='description' name='description'
        placeholder='Add task description' />
    </label>
    <label>
      <span>Task Date & Time</span>
      <input type='date' placeholder="add day" required v-model="date" name='date' />
      <input type='time' placeholder="add time" required v-model="time" name='time' />
    </label>
    <div class="flex justify-between items-center">
      <span>Set reminder</span>
      <ToggleComponent @change-event="e => this.reminder = e.target.checked" :isChecked='reminder' name='reminder' />
    </div>
    <Button classes="block w-full my-4 rounded-md px-3 py-3.5 uppercase text-lg font-bold bg-black text-white"
      text='submit' type='submit' />
  </form>
</template>

<style scoped>
form>label {
  display: flex;
  flex-direction: column;
  row-gap: 0.5rem;
  margin: 1.2rem auto;
}

label input {
  width: 100%;
  border: 1px solid #191919da;
  padding: .5rem 1rem;
  border-radius: 5px;
  color: #191919da;
}

label input::placeholder::first-letter {
  color: #000000ac;
}

label input:focus {
  border: 1px dotted #191919da;
  outline: none;
}

label span,
div span {
  font-size: 1.2rem;
  color: rgb(26, 26, 26);
  display: block;
  margin-bottom: -0.25rem;
}
</style>
