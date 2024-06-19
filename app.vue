<template>
    <div class="main">
        <Header title="Rhyming Sentence Generator" />
        <div class="content">
            <Subheader text="Input" />
            <input type="text" v-model="word" maxlength="15" placeholder="Enter the word to rhyme with" />
            <button v-on:click="generate()" :disabled="loading == true">
                <Spinner class="button-spinner" v-if="loading == true" />
                <p v-else>Generate</p>
            </button>

            <Divider />

            <Subheader text="Output" />
            <div v-show="output != '' || loading == true" class="output">
                <Spinner v-if="loading == true" />
                <p>{{ output }}</p>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
// get font
useHead({
    title: 'Rhyming Sentence Generator',
    link: [
        {
            rel: 'preconnect',
            href: 'https://rsms.me/'
        },
        {
            rel: 'stylesheet',
            href: 'https://rsms.me/inter/inter.css'
        }
    ],
    meta: [
        {
            name: 'viewport',
            content: 'width=device-width, initial-scale=1.0'
        },

        {
            name: 'og:title',
            content: 'Rhyming Sentence Generator'
        },
        {
            name: 'og:description',
            content:
                'On the declining alpine incline, nine divine felines dined on fine wine and spiced pine, intertwining their feline minds to align with the starry sign, entwined in the confines of the moonlit shrine, as the night became benign and they reclined, basking in twilight’s crystalline shine.'
        },
        {
            name: 'og:image',
            content: 'https://rhymes.coolpixels.net/icon.webp'
        },
        {
            name: 'og:url',
            content: 'https://rhymes.coolpixels.net/'
        }
    ]
});

// create refs
const word = ref('');
const output = ref('');
const loading = ref(false);

// generate function
async function generate() {
    // clear output
    output.value = '';

    // mark as loading
    loading.value = true;

    // generate sentence
    const response = await fetch(`https://api.coolpixels.net/generateRhymingSentence/${word.value}`);
    const data: { success: boolean; data: string | undefined } = await response.json();

    if (data.success) {
        output.value = data.data || '';
    } else {
        output.value = 'An error occurred while generating the sentence';
    }

    // mark as not loading
    loading.value = false;
}
</script>

<style>
:root {
    font-family: InterVariable, sans-serif;
    font-feature-settings: 'liga' 1, 'calt' 1;

    background-color: #141414;
    color: #ffffff;
}

h1,
h2,
h3,
h4,
h5,
h6,
p {
    margin: 0;
}

input,
button,
.output {
    height: 48px;
    font-size: inherit;
    font-family: inherit;
    color: inherit;
    border-style: solid;
    border-width: 4px;
    border-radius: 24px;
    padding: 12px;
    border-color: #414141;
}

input,
.output {
    background-color: #232323;
}

.output {
    height: min-content;
}

button {
    width: 25% !important;
    cursor: pointer;
    background: linear-gradient(to bottom, #00bcd1, #0073d2);
}

button[disabled] {
    cursor: not-allowed;
    background: linear-gradient(to bottom, #6f747f, #474a54);
}

.content {
    width: 50%;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 16px;
}

.content > * {
    width: 100%;
    min-width: 320px;
    max-width: 512px;
}

.button-spinner {
    height: 100% !important;
}
</style>
