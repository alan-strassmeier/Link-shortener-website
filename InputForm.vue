<script setup>
import ButtonGenerate from './ButtonGenerate.vue';
import { computed, ref } from 'vue';
import axios from 'axios';
import { useToast } from "vue-toastification";

const urlInput = ref(null);
const shortenedUrl = ref('');
const toast = useToast();

const displayClass = computed(() => shortenedUrl.value === '' ? 'display' : '');

const BITLY_ACCESS_TOKEN = '7817224901ceaceae91f52d86673d7e2d2d6af69';

const isValidURL = (url) => {
    try {
        new URL(url);
        return true;
    } catch (_) {
        return false;
    }
};

const pasteContent = async () => {
    if (!navigator.clipboard || !navigator.clipboard.readText) {
        toast.error('The Clipboard API is not supported or available in your browser.');
        return;
    }

    try {
        const contentToPaste = await navigator.clipboard.readText();
        if (urlInput.value) {
            urlInput.value.value = contentToPaste;
        }
    } catch (error) {
        toast.error('Unable to access clipboard. Check browser permissions.');
    }
};

const copyContent = async () => {
    const text = shortenedUrl.value;
    try {
        await navigator.clipboard.writeText(text);
        toast.success('Copied to clipboard!');
    } catch (err) {
        toast.error('Failed to copy text to clipboard.');
        console.error('Failed to copy text to clipboard: ', err);
    }
};


const shortenURL = async (url) => {
    try {
        const response = await axios.post(
            'https://api-ssl.bitly.com/v4/shorten',
            { long_url: url },
            {
                headers: {
                    'Authorization': `Bearer ${BITLY_ACCESS_TOKEN}`,
                    'Content-Type': 'application/json',
                },
            }
        );
        return response.data.link;
    } catch (error) {
        toast.error('Error shortening the URL. Please try again.');
        console.error('Error from Bitly API:', error);
        return null;
    }
};

const handleGenerateClick = async () => {
    const url = urlInput.value ? urlInput.value.value : '';
    if (isValidURL(url)) {
        toast.success('URL is valid!');
        const shortUrl = await shortenURL(url);
        if (shortUrl) {
            shortenedUrl.value = shortUrl;
        }
    } else {
        toast.error('Invalid URL. Please enter a valid URL.');
    }
};
</script>

<template>
    <div class="container__center">
        <div class="container__form">
            <form @submit.prevent="handleGenerateClick">
                <label for="url"></label>
                <div class="input__wrapper">
                    <input ref="urlInput" class="url__input" type="text" id="url" name="url"
                        placeholder="https://www.example.co">
                    <button type="button" class="paste__button" @click="pasteContent">
                        <img src="../assets/images/copy.png" alt="Paste" class="paste__image">
                    </button>
                </div>
            </form>
            <ButtonGenerate @click="handleGenerateClick" />
            <div class="shorten__container">
                <p>Link Shorten:</p>
            </div>
            <div :class="['output__area', displayClass]">
                <button type="button" class="paste__button" @click="copyContent">
                    <img src="../assets/images/copy.png" alt="Copy Button" class="paste__image">
                </button>
                <span>{{ shortenedUrl }}</span>
            </div>
        </div>
    </div>
</template>

<style scoped>
.container__center {
    position: absolute;
    width: 100vw;
    margin-top: 50px;
}

.container__form {
    height: 29rem;
    width: 52.5rem;
    background-color: #8EC4E3;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    margin: auto;
    border-radius: 5px;
}

.input__wrapper {
    position: relative;
    width: 555px;
    margin-top: 55px;
}

.url__input {
    border: none;
    height: 65px;
    width: 100%;
    font-size: 24px;
    padding-left: 20px;
    box-sizing: border-box;
}

.paste__button {
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
    padding: 0;
    border: none;
    background: none;
    cursor: pointer;
}

.paste__image {
    width: 24px;
    height: 24px;
}

.shorten__container {
    top: 330px;
    position: absolute;
    width: 840px;
    height: 135px;
    background-color: #53778C;
    border-radius: 0 0 5px 5px;
    display: flex;
    justify-content: center;
}

p {
    margin-top: 15px;
    font-size: 24px;
    color: black;
}

span {
    margin: auto;
}

.output__area {
    position: relative;
    margin-top: 185px;
    background-color: white;
    border: none;
    height: 65px;
    width: 65%;
    font-size: 24px;
    padding-left: 20px;
    box-sizing: border-box;
    display: flex;
}

.display {
    display: none
}


@media screen and (max-width: 900px) {
    .shorten__container {
        top: 24rem;
    }

    .container__form {
        height: 33rem;
    }
}
</style>
