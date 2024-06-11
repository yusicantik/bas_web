<template>
    <div>
        <h1> {{ text }} </h1>
    </div> 
    <slot/>
    <slot name="part1_header" :param1="data.param_from_slot"></slot>
</template>
<script setup>

import {reactive} from 'vue';

const data = reactive({
    param_from_slot: 'Hello from Part One Header' 
})

defineProps({
    text: {
        type: String,
        default: ''
    }
})
</script>