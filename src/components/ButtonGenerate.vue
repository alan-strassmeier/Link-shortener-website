<script setup>
const emit = defineEmits(['click']);
</script>

<template>
    <button type="button" class="btn__form" @click="$emit('click')">Generate</button>
</template>

<style scoped>
.btn__form {
    font-size: 24px;
    height: 60px;
    width: 250px;
    background-color: #F4A460;
    margin-top: 20px;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
    border: none;
}

.btn__form:active {
    box-shadow: none;
}
</style>