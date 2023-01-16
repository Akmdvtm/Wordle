<script setup>
import SimpleKeyboard from "./components/SimpleKeyboard.vue";
import {onMounted, reactive} from "vue";

const handleInput = (key) => {
  console.log(key)
};

const state = reactive({
  solution: "books",
  guesses: ["", "", "", "", "", ""],
  currentGuessIndex: 0,
})

onMounted(() => {
  window.addEventListener("keyup", (e) => {
    e.preventDefault()
    let key =
        e.key === "Enter"
            ? "{enter}"
            : e.key === "Backspace"
            ? "{bksp}"
            : e.key.toLowerCase()
            // : String.fromCharCode(e.code).toLowerCase()
    handleInput(key)
  })
})
</script>

<template>
  <div class="flex flex-col h-screen max-w-md mx-auto justify-evenly">
    <simple-keyboard @onKeyPress="handleInput"/>
  </div>
</template>

<style scoped>
</style>
