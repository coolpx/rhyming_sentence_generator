<template>
    <div class="root">
        <div class="container">
            <img src="/spinner.svg" />
        </div>
    </div>
</template>

<style scoped>
.root {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 64px;
}

.container {
    width: 64px;
    height: 100%;
}

img {
    filter: invert(100%);

    width: 100%;
    height: 100%;

    animation: spin 1s linear infinite;
}

@keyframes spin {
    0% {
        transform: rotate(0deg);
    }
    100% {
        transform: rotate(360deg);
    }
}
</style>
