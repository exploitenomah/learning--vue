

<script>
import Toggler from "./Toggler.vue";
export default {
    name: "TaskItemComponent",
    props: {
        task: {
            type: Object,
            default() {
                return {
                    description: "",
                    date: "",
                    reminder: false,
                    time: "",
                    id: Math.random
                };
            }
        }
    },
    emits: [
        "delete-me",
        "toggle-my-reminder"
    ],
    components: { Toggler }
}

</script>

<template>
    <div
        :class='[task.reminder ? " border-l-blue-400" : "border-l-transparent", " px-3 py-2 bg-gray-100 rounded-lg border-l-4"]'>
        <div :class='["flex items-baseline justify-between"]'>
            <div>
                <h3 class="text-xl font-semibold">{{ task.description }}
                </h3>
                <p class='mt-3 flex flex-col gap-y font-medium justify-start items-start '>
                    <span>{{ task.date }}</span>
                    <span>{{ task.time }}</span>
                </p>
            </div>
            <button className='text-red-400 text-xl font-bold lowercase' @click='$emit("delete-me")'>&#215;
            </button>
        </div>
        <div :class='[task.reminder ? "text-green-600" : "text-slate-400", " flex grow justify-between items-center"]'>
            Remind me
            <Toggler name=' checked' :isChecked='task.reminder' @change-event="$emit('toggle-my-reminder')" />
        </div>
    </div>
</template>

<style scoped></style>
