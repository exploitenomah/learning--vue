
<script>
import Button from './Button.vue'
export default {
  name: 'HeaderComponent', props: { title: String },
  components: {
    Button
  },
  methods: {

  }
}

</script>

<template>
  <header>
    <h1>{{ title }}</h1>
    <Button text='Add Task' color='green' @on-click="$emit('toggle-show-form')" />
  </header>
</template>

<style scoped>
header {
  display: flex;
  justify-content: space - between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
