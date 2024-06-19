<template>
    <div>
        <p>{{ props.label }}</p>
        <input type="checkbox" :checked="props.checked" @change="handleChange" />
    </div>
</template>

<script setup lang="ts">
const props = defineProps<{
    label: string;
    checked: boolean;
}>();

const emit = defineEmits<{
    (event: 'change', checked: boolean): void;
}>();

function handleChange(event: Event) {
    emit('change', (event.target as HTMLInputElement).checked);
}
</script>

<style scoped>
div {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 32px;
}

div > * {
    height: 32px;
    margin: 0;
}

p {
    flex-grow: 1;
    font-size: 24px;
    font-weight: 500;
}

input {
    width: 24px;
    border: 4px solid #414141;
    padding: 6px 12px;
}
</style>
