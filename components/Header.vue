<template>
    <div>
        <h1>{{ title }}</h1>
    </div>
</template>

<script setup lang="ts">
defineProps<{
    title: string;
}>();
</script>

<style scoped>
div {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0;
    margin-bottom: 16px;
    padding: 0;
}

h1 {
    font-size: 400%;
    text-align: center;
    background: linear-gradient(to bottom, #ffffff, #49465b);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
}
</style>