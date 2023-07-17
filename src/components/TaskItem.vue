
<script>
export default {
    name: 'TaskItemComponent',
    props: {
        task: {
            type: Object,
            default() {
                return {
                    description: '',
                    date: '',
                    reminder: false,
                    time: '',
                    id: Math.random
                }
            }
        }
    },

}

</script>

<template>
    <div>
        <h3>{{ task.description }}
        </h3>
        <p>
            <span>{{ task.date.toString() }}</span>
            <span>{{ task.time }}</span>
        </p>
    </div>
</template>

<style scoped></style>