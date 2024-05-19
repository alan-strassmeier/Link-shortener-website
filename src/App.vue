<template>
  <div>
    <h1> URL Shortener</h1>
    <InputForm />
    <BgStyle />
  </div>
</template>

<script setup>
import BgStyle from "./components/BgStyle.vue"
import InputForm from "./components/InputForm.vue"
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Libre+Barcode+39+Extended+Text&display=swap');

h1 {
  font-family: "Libre Barcode 39 Extended Text", system-ui;
  font-size: 96px;
  font-weight: 400;
  font-style: normal;
  text-align: center;
  margin-top: 35px;
}
</style>
