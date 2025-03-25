<script setup>
import {ref, computed} from 'vue'
import { generate} from 'random-words'

const livesCounter = ref(10)
let correctWord = Array.from(generate({ minLength: 5, maxLength: 7 }))
console.log(correctWord)
const res = ref(Array.from(new Array(correctWord.length)).fill(''))
const lettersChecked = ref([])
const inputLetter = ref('')
const isFinish = computed(() => {
  for (var i = 0; i < res.value.length; ++i) {
    if (res.value[i] !== correctWord[i]) return false
  }
  return true
})

function checkLetter(letter){
  if (isFinish.value || livesCounter.value <= 0){
    inputLetter.value = ''
    return;
  }
  if (lettersChecked.value.includes(letter)){
    inputLetter.value = ''
    return;
  }
  lettersChecked.value.push(letter)
  inputLetter.value = ''
  if (correctWord.includes(letter)){
    correctWord.forEach((l, i) => {
      if (l == letter) {
        res.value[i] = letter
      }
    })
  } else {
    livesCounter.value -= 1
  }
}

function playAgain(){
  if (livesCounter.value > 0)
    correctWord = Array.from(generate({ minLength: 5, maxLength: 7 }))
  console.log(correctWord)
  res.value = Array.from(new Array(correctWord.length)).fill('')
  lettersChecked.value = []
  livesCounter.value = 10
}

</script>

<template>
  <div>
    <div> 
      <div class="lives_counter">
        <p>Lives left: </p>
        <div :style="{color: (livesCounter <= 0) ? 'red' : 'grey'}">{{ livesCounter }}</div>
      </div>
      <h3>Enter the letter</h3>
      <input v-model="inputLetter" v-on:keyup.enter="checkLetter(inputLetter)" class="input_l" type="text" maxlength="1">
    </div>
    <div id="letters">
      <div class="letter" v-for="l in res">
        {{l}}
      </div>
    </div>

    <div class="lsChecked" :style="{width: `${correctWord.length * 60}px`}">
      <div>Entered letters:</div>
      <div class="lChecked" v-for="lC in lettersChecked" :style="{color: (correctWord.includes(lC) ? 'rgb(0, 200, 0)':'rgb(200, 0, 0)')}">
        {{ lC }}
    </div>
  </div>
  <div class="finishW" v-if="isFinish">
    <p>You win!!!</p>
    <button v-on:click="playAgain">Play again</button>
  </div>
  <div class="finishL" v-if="livesCounter <= 0">
    <p>You lose</p>
    <button v-on:click="playAgain">Play again</button>
  </div>
  </div>


</template>

<style>

.lives_counter{
  display: flex;
  gap: 10px;
  font-size: 24px;
}
#letters {
  margin-top: 30px;
  height: 60px;
  display: flex;
}

.letter{
  box-sizing: border-box;
  display: inline-block;
  border: solid white 1px;
  height: 60px;
  width: 60px;
  font-size: 32px;
  text-align: center;
}

.input_l{
  background-color: rgb(91, 91, 91);
  height: 58px;
  width: 58px;
  font-size: 30px;
  text-align: center;
  color: white;
}

.lsChecked{
  margin-top: 10px;
  display: flex;
  flex-wrap: wrap;
}

.lChecked{
  color: white;
  margin-left: 5px;
  margin-right: 5px;
}

.finishW{
  font-size: 50px;
  color: green;
}

.finishL{
  font-size: 50px;
  color: rgb(166, 27, 27);
}

</style>
