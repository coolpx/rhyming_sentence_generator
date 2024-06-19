<template>
    <div v-show="open" class="root">
        <div class="notification">
            <div class="content">
                <p>{{ content }}</p>
            </div>
            <img v-on:click="close()" src="/xmark.svg" />
        </div>
        <div v-if="includeSpacer" style="height: 64px" />
    </div>
</template>

<script setup lang="ts">
// define props
const props = defineProps<{
    content: string;
    includeSpacer?: boolean;
    id: any;
}>();

// create refs
const open = ref(true);

// close function
function close() {
    open.value = false;
}

// watch props
watch(
    () => [props.id],
    () => {
        open.value = true;
    }
);
</script>

<style scoped>
.notification {
    width: 100vw;
    height: 64px;
    left: 0;
    top: 0;
    position: fixed;
    background-color: rgb(255, 39, 50);

    display: flex;
    justify-content: center;
    align-items: center;
}

.content {
    width: 100%;
    margin: 0 64px;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

p {
    font-size: 150%;
}

img {
    filter: invert(100%);
    height: 32px;
    position: fixed;
    right: 16px;
    top: 16px;
    cursor: pointer;
}
</style>
