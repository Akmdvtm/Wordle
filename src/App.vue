<script setup>
import SimpleKeyboard from "./components/SimpleKeyboard.vue";
import {computed, onMounted, reactive} from "vue";
import WordRow from "./components/WordRow.vue";

const state = reactive({
  solution: "books",
  guesses: ["", "", "", "", "", ""],
  currentGuessIndex: 0,
})

const wonGame = computed(
    () => state.guesses[state.currentGuessIndex - 1] === state.solution
)

const lostGame = computed(() => !wonGame.value && state.currentGuessIndex >= 6)
const handleInput = (key) => {
  if (state.currentGuessIndex >= 6 || wonGame.value) {
    return
  }

  const currentGuess = state.guesses[state.currentGuessIndex]

  if (key === "{enter}") {
    if (currentGuess.length === 5) {
      state.currentGuessIndex++
    }
  } else if (key === "{bksp}") {
    state.guesses[state.currentGuessIndex] = currentGuess.slice(0, -1)
  } else if (currentGuess.length < 5) {
    const alphaRegex = /^[a-zA-Z]+$/;
    if (alphaRegex.test(key)){
      state.guesses[state.currentGuessIndex] += key
    }
  }
};

onMounted(() => {
  window.addEventListener("keyup", (e) => {
    e.preventDefault()
    let key =
        e.key === "Enter"
            ? "{enter}"
            : e.key === "Backspace"
            ? "{bksp}"
            : e.key.length === 1
            ? e.key.toLowerCase()
            : ""
    handleInput(key)
  })
})
</script>

<template>
  <div class="flex flex-col h-screen max-w-md mx-auto justify-evenly">
    <div>
      <word-row
          v-for="(guess,i) in state.guesses"
          :key="i"
          :value="guess"
          :solution="state.solution"
          :submitted="i < state.currentGuessIndex"
      />
    </div>
    <p v-if="wonGame" class="text-center flex flex-col items-center">
      🏆 Congrats you solved it!
      <button
          class="p-2 mt-2 border-green-500 rounded-md bg-green-500 text-white"
      >New game
      </button>
    </p>
    <p v-else-if="lostGame" class="text-center flex flex-col items-center">
      ☠️ Out of tries.
      <button
          class="p-2 mt-2 border-gray-500 rounded-md bg-gray-500 text-white"
      >New game
      </button>
    </p>
    <simple-keyboard
        @onKeyPress="handleInput"
    />
  </div>
</template>

<style scoped>
</style>
