<template>
    <button :type='type' @click="this.$emit('on-click')" :class="[bgColorClass, classes]">{{ text }}
    </button>
</template>
<script>
export default {
    name: 'ButtonComponent',
    props: {
        text: String,
        bgColorClass: String,
        classes: String,
        type: {
            type: String,
            default: 'button'
        }
    },
}
</script>

