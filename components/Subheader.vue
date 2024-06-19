<template>
    <h2>{{ text }}</h2>
</template>

<script setup lang="ts">
defineProps<{
    text: string;
}>();
</script>

<style scoped>
h2 {
    text-align: left;
    width: 100%;
}
</style>