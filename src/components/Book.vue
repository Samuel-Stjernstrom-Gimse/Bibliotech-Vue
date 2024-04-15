<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps({
    title: {
        type: String
    },
    author: {
        type: String
    },
    year: {
        type: Number
    },
    id: {
        type: String
    }
})
</script>

<template>
    <div>
        <h4>{{ props.title }}</h4>
        <h4>{{ props.id }}</h4>
        <h4>{{ props.author }}</h4>
        <h4>{{ props.year }}</h4>
    </div>
</template>

<style scoped></style>
