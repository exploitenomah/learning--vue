<template>
    <button :type='type' @click="this.$emit('on-click')" :style="{ backgroundColor: color }">{{ text }}
    </button>
</template>
<script>
export default {
    name: 'ButtonComponent',
    props: {
        text: String,
        color: String,
        type: {
            type: String,
            default: 'button'
        }
    },
}
</script>

