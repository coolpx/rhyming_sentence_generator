<template>
    <div class="main">
        <Notification v-if="notification != ''" :content="notification" :id="notificationId" includeSpacer />

        <Header title="Rhyming Sentence Generator" />
        <div class="content">
            <Subheader text="Input" />
            <input type="text" v-model="word" maxlength="15" placeholder="Enter the word to rhyme with" />
            <Checkbox label="High quality" :checked="highQuality" @change="setHighQuality" />
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
const notification = ref('');
const notificationId = ref(0);
const highQuality = ref(false);

// change quality function
function setHighQuality(checked: boolean) {
    highQuality.value = checked;
}

// format time
function formatTimeFromMilliseconds(milliseconds: number): string {
    // Calculate hours, minutes, and seconds
    const hours = Math.floor(milliseconds / (1000 * 60 * 60));
    const minutes = Math.floor((milliseconds % (1000 * 60 * 60)) / (1000 * 60));
    const seconds = Math.floor((milliseconds % (1000 * 60)) / 1000);

    // Format minutes and seconds to be always two digits
    const formattedMinutes = minutes < 10 ? `0${minutes}` : minutes;
    const formattedSeconds = seconds < 10 ? `0${seconds}` : seconds;

    // Combine hours, formatted minutes, and formatted seconds into a time string
    return `${hours}:${formattedMinutes}:${formattedSeconds}`;
}

// generate function
async function generate() {
    // increment notification id
    notificationId.value++;

    // clear notification
    notification.value = '';

    // mark as loading
    loading.value = true;

    // validate word
    if (word.value.match(/[^a-zA-Z\s]/)) {
        notification.value = 'Word can only contain alphabetical characters';
        loading.value = false;
        return;
    }

    const strippedWord = word.value.replace(/^\s+|\s+$/g, '').replace(/\s+/g, ' ');

    if (strippedWord.length < 2) {
        notification.value = 'Word cannot have less than 2 characters';
        loading.value = false;
        return;
    } else if (strippedWord.length > 15) {
        notification.value = 'Word cannot have more than 15 characters';
        loading.value = false;
        return;
    }

    // clear output
    output.value = '';

    // create url
    const url = new URL(
        `https://api.coolpixels.net/generateRhymingSentence/${
            (highQuality.value && 'highQuality/') || ''
        }${strippedWord}`
    );

    // generate sentence
    const response = await fetch(url);
    const data: { success: boolean; data?: string; error?: string; retryAfter?: number } = await response.json();

    if (data.success) {
        output.value = data.data || '';
    } else {
        if (response.status === 429) {
            notification.value = `Please try again in ${formatTimeFromMilliseconds(
                data.retryAfter || 0
            )} or try a different quality setting.`;
        } else if (response.status === 400) {
            notification.value = data.error || 'An error occurred while generating the sentence.';
        } else if (response.status === 500) {
            notification.value = 'A server error occurred while generating the sentence.';
        } else {
            notification.value = data.error || 'An unknown error occurred while generating the sentence.';
        }
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
    padding: 6px 12px;
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
