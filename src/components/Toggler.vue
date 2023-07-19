<template>
    <label :class='[isChecked ? "bg-blue-100" : "bg-slate-200", "relative rounded-full h-[1.85rem] w-[55px]"]'>
        <input type='checkbox' @change='e => $emit("change-event", e)' :value='isChecked' class="hidden" :name='name' />
        <span :class='[isChecked ? "translate-x-[98%] bg-blue-500 " : "translate-x-[25%] bg-slate-300 "
            , "z-inherit top-[50%] translate-y-[-50%] block rounded-full absolute w-[1.5rem] h-[1.5rem] transition-all"]
            '></span>
    </label>
</template>
<script>
export default {
    name: 'ToggleComponent',
    props: {
        name: String,
        isChecked: Boolean
    },

}
</script>
