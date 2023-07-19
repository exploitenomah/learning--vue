
<script>
import Button from './Button.vue'
export default {
  name: 'HeaderComponent', props: { title: String, showForm: Boolean },
  components: {
    Button
  },
}

</script>

<template>
  <header class='sticky z-10 top-0 bg-white flex w-full border-b justify-between items-center max-h-[12vh] px-4 py-5'>
    <h1 class='text-4xl font-normal capitalize tracking-normal'>{{ title }}</h1>
    <Button :text='showForm ? "Close" : "Add Task"' :bgColorClass='showForm ? "bg-red-500" : "bg-green-600"'
      classes='text-white px-3 py-2 rounded-md capitalize text-sm font-semibold whitespace-nowrap  transition-all duration-[500ms] '
      @on-click="$emit('toggle-show-form')" />
  </header>
</template>

