
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
    emits: [
        "delete-me", 'toggle-my-reminder'
    ]
}

</script>

<template>
    <div>
    </div>
    <h3>{{ task.description }}
    </h3>
    <p>
        <span>{{ task.date }}</span>
        <span>{{ task.time }}</span>
    </p>

    <div>
        <button @click='$emit("delete-me")'>delete
        </button>
    </div>
    <label>
        <input type='checkbox' :checked='task.reminder' @click="$emit('toggle-my-reminder')" />
    </label>
</template>

<style scoped></style>
