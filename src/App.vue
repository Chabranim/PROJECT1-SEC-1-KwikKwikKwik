<script setup>
// function ที่เหลือ ให้นับเวลาหลังจากกด play
import { reactive, ref } from "vue"
import { onMounted, onBeforeMount } from 'vue'


import MaterialSymbolsCancel from "../src/assets/MaterialSymbolsCancel.vue"
import PepiconsPopPaintPallet from "../src/assets/PepiconsPopPaintPallet.vue"

const data = reactive({
  words: ["RED", "BLUE", "GREEN", "YELLOW", "PINK", "ORANGE", "BLACK", "PURPLE"],
  colors: ["black", "blue", "pink", "purple", "orange", "yellow", "red"],
  randomColorName: '',
  randomColor: ''
})

let timer = ref(0);
// const countdownTime = ref(null);
let score = ref(0)
const gameInProgress = ref(false)
const gameOver = ref(true)

// 1
const getRandomIndex = (max) => {
  return Math.floor(Math.random() * max);
}

const generateRandomColor = () => {
  data.randomColorName = data.words[getRandomIndex(data.words.length)];
  data.randomColor = data.colors[getRandomIndex(data.colors.length)];
  // console.log(data.randomColorName);
  // console.log(data.randomColor);
}

// let correct = ref()
// 2
const checkColor = (color) => {
  console.log(color);
  if (color.toLowerCase() === data.randomColor) {
    // correct.value = true

    // console.log(correct.value);
    score.value++
  } else {
    // correct.value = false
    // console.log(correct.value);
    if (score < 0) {

      score.value--
    }
  }
  generateRandomColor()
}
// console.log(checkColor());



onMounted(() => {
  generateRandomColor();
})
// 3
const startCountdownTimer = () => {
  const countdownInterval = setInterval(() => {
    if (timer.value === 0) {
      clearInterval(countdownInterval)
      gameOver.value = true
    } else {
      timer.value--
    }
  }, 1000);
}
// 4
const startGame = () => {
  gameInProgress.value = true;
  score.value = 0
  timer.value = 10
  generateRandomColor()
  startCountdownTimer()
}

const gameOverEnd = () => {
  gameOver.value = false
  score.value = 0
  timer.value = 10
  generateRandomColor()
  startCountdownTimer()
}


const closeTheGame = () => {
  gameInProgress.value = false
  // score.value = 0
  // timer.value = 10
  // generateRandomColor()

}
const namePlayer = ref('')
const playerPage = ref(false)

const namePlayerPage = () => {
  playerPage.value = true
}

</script>

<template>
  <!-- default -->
  <div class="w-full h-screen pb-3 bg-[url('../src/assets/FluentEmojiFlatPaintbrush.svg')]">
    <div class="bg-[#67274C] p-4 absolute w-full">
      <div class="flex items-center text-[#E9BCB9] space-x-2 justify-center">
        <span class="text-[#E9BCB9] text-4xl ml-5 font-extrabold">Click the color
          <span class=" rounded-sm p-0.5 stroke text-red-600">NOT</span> the word</span>
        <PepiconsPopPaintPallet />
      </div>
    </div>
    <!-- tag side -->
    <div class="flex space-x-3 pr-24 pl-24 pt-24 h-full pb-1">


      <div class="w-full bg-[#312C58] rounded-xl">
        <div class="flex bg-[#1D1A39] rounded-t-xl justify-end">
          <div class="flex space-x-2 justify-end m-2 p-1">
            <span class="dot bg-[#8FFF5A]"></span>
            <span class="dot bg-[#FB6161]"></span>
            <span class="dot bg-[#FBFE6D]"></span>

          </div>
        </div>
        <div class=" color2" v-if="!gameInProgress">
          <div class="space-x-2">
            <span class="dot1 bg-black "></span>
            <span class="dot1 bg-purple-600"></span>
            <span class="dot1  bg-pink-400"></span>
            <span class="dot1 bg-[#8FFF5A]"></span>
            <span class="dot1 bg-[#FB6161]"></span>
            <span class="dot1 bg-[#FBFE6D]"></span>
            <span class="dot1 bg-blue-600"></span>
            <span class="dot1 bg-orange-400 "></span>
          </div>
          <button v-if="!playerPage" @click="namePlayerPage" class=" border border-black buttonClick ">Play</button>

        </div>

        <!-- เขียนชื่อ -->
        <div v-if="playerPage && !gameInProgress">
          <div class="w-full h-full">
            <div class="flex flex-col items-center space-y-20 space-x-1">

              <div class="text-white text-4xl ">Enter you name</div>

              <span><input type="text" class="text-3xl" v-model="namePlayer"></span>
              <button @click="startGame" class=" border border-black buttonClick ">Next</button>
              <div class="mt">
                <label for="my_modal_6" class="btn">How to play ❔</label>
                <input type="checkbox" id="my_modal_6" class="modal-toggle" />
                <div class="modal">
                  <div class="modal-box">
                    <h3 class="font-bold text-lg text-center">Instruction</h3>
                    <p class="py-4">
                    <div class="list-decimal">
                      <li>Once the game starts, the timer will begin counting down from a certain value (e.g., 5
                        seconds).
                      </li>
                      <li>
                        Look at the words displayed on the screen. Each word is written in a specific color.
                      </li>
                      <li>
                        Your goal is to click on the word that matches the color of the text, not the text itself. For
                        example:
                        - If the word "Red" is displayed in blue color, click on the word "Red."
                        - If the word "Green" is displayed in green color, click on the word "Green."
                      </li>
                      <li>
                        Continue making selections as quickly as possible within the time limit.
                      </li>
                      <li>
                        You earn one point for each correct selection.
                      </li>
                      <li>
                        The game ends when the timer reaches 0.
                      </li>
                      <div>
                        <h1 class="font-bold text-lg ">Tips:</h1>
                        <li>Focus on the color of the text rather than reading the word.</li>
                        <li>Try to make quick decisions to maximize your score within the time limit.</li>
                      </div>
                    </div>

                    </p>
                    <div class="modal-action">
                      <label for="my_modal_6" class="btn">Close!</label>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>






        <div v-if="gameInProgress">
          <!-- <h1 v-show="correct == true"> Great !</h1> -->
          <div class=" justify-end text-red-500">
            <button @click="closeTheGame">
              <MaterialSymbolsCancel />
            </button>
          </div>
          <div v-show="timer > 0">
            <h1 class=" bg-white">Name : {{ namePlayer }}</h1>
            <h1 class=" bg-white">Score: {{ score }}</h1>
            <h1 class=" bg-white">Timer : {{ timer }}</h1>
            <div class=" color" :style="{ 'background-color': 'white', color: data.randomColor }">{{ data.randomColorName
            }}
            </div>
            <div class="flex space-x-4 px-4 justify-center mt-9">
              <div v-for="color in data.words">
                <button @click="checkColor(color)" class="py-2 rounded-md buttonClick "
                  :style="{ 'background-color': color.toLowerCase(), 'color': 'white', 'font-weight': 'bold' }">{{ color
                  }}</button>
              </div>
            </div>
          </div>
          <div class=" color2 bg-white" v-if="timer === 0">
            Game over !
            <div>{{ namePlayer }} score : {{ score }}</div>
            <button class=" border border-black buttonClick " @click="gameOverEnd">Try again ?</button>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<style>
@font-face {
  font-family: "dbheavent";
  src: url("./fonts/DBHeaventBd/DBHeavent.ttf") format("truetype");
}

/* .buttonPlay{
  
} */

.buttonClick {
  border-radius: 12px;
  background-color: white;
  padding: 10px;
  margin: 6px;
}

.dot {
  height: 20px;
  width: 20px;
  border-radius: 50%;
  display: inline-block;
}

.dot1 {
  height: 70px;
  width: 70px;
  border-radius: 50%;
  display: inline-block;
}



.centerdiv {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.color {
  font-size: 170px;
  text-align: center;
  padding: 2px;
  margin-top: 4%;
  margin-left: 25%;
  margin-right: 25%;
  border-radius: 25px;
  -webkit-text-stroke: 2px black;
}

.stroke {
  -webkit-text-stroke: 1.5px white;
}

.color2 {
  font-size: 50px;
  text-align: center;
  padding: 2px;
  margin-top: 4%;
  margin-left: 20%;
  margin-right: 20%;
  border-radius: 25px;
  -webkit-text-stroke: 2px black;
}
</style>
