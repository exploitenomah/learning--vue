
<script>
import Button from './Button.vue'
export default {
  name: 'TaskFormComponent',
  components: {
    Button,
  },
  props: {
    show: Boolean,
    formValues: {
      type: Object,
      default() {
        return {
          date: '',
          description: '',
          reminder: false
        }
      }
    }
  },
  methods: {
    handleChange(e) {
      this.$emit('change-event', e)
    },
    handleSubmit(e) {
      this.$emit('submit-event', e)
    }
  }
}

</script>

<template>
  <form v-show="show" @submit="handleSubmit">
    <label>
      <span>Task Description</span><input type='text' required :value='formValues.description' name='description'
        @change="handleChange" placeholder='add task description' />
    </label>
    <label>
      <span>Task Date & Time</span>
      <input type='date' placeholder="add day" required :value="formValues.date" name='date' @change="handleChange" />
      <input type='time' placeholder="add time" required :value="formValues.time" name='time' @change="handleChange" />
    </label>
    <label> <span>Set reminder</span><input type='checkbox' :value='formValues.reminder' name='reminder'
        @change="handleChange" /></label>
    <Button text='submit' type='submit' />
  </form>
</template>

<style scoped></style>
