<script setup>
// function ที่เหลือ ให้นับเวลาหลังจากกด play
import { reactive, ref } from "vue";
import { onMounted } from "vue";

import MaterialSymbolsCancel from "../src/assets/MaterialSymbolsCancel.vue";
import PepiconsPopPaintPallet from "../src/assets/PepiconsPopPaintPallet.vue";
import TwemojiArtist from "../src/assets/TwemojiArtist.vue"
import TwemojiArtistPalette from "../src/assets/TwemojiArtistPalette.vue"
import TwemojiBackhandIndexPointingDownMediumDarkSkinTone from "../src/assets/TwemojiBackhandIndexPointingDownMediumDarkSkinTone.vue"

const data = reactive({
  words: [
    "RED",
    "BLUE",
    "GREEN",
    "YELLOW",
    "PINK",
    "ORANGE",
    "BLACK",
    "PURPLE",
  ],
  colors: ["black", "blue", "pink", "purple", "orange", "yellow", "red"],
  randomColorName: "",
  randomColor: "",
});

let score = ref(0);
let timer = ref(0);
const gameInProgress = ref(false);
const namePlayer = ref("");
const playerPage = ref(false);
const progressWidth = ref("");
const modeGame = ref(false);
const timeOfMode = ref(0);
let shuffledArray = ref([]);
const valueOfMode = ref('')


const getRandomIndex = (max) => {
  return Math.floor(Math.random() * max);
};

const generateRandomColor = () => {
  data.randomColorName = data.words[getRandomIndex(data.words.length)];
  data.randomColor = data.colors[getRandomIndex(data.colors.length)];
};




const randomButton = () => {
  shuffledArray.value = data.words 
    .map((value) => ({ value, sort: Math.random() })) 
    .sort((a, b) => a.sort - b.sort)
    .map(({ value }) => value);
};



const checkColor = (color) => {
  console.log(color);
  if (color.toLowerCase() === data.randomColor) {
    score.value++;
  } else {
    if (score <= 0) {
      score.value = 0
    }
  }
  generateRandomColor();
  randomButton();
};




onMounted(() => {
  generateRandomColor();
  randomButton();
});



const startCountdownTimer = () => {
  let countdownTimer = setInterval(() => {
    timer.value--;
    if (timer.value >= 0) {
      progressWidth.value = (timer.value / timeOfMode.value) * 100 + "%";

    }else if(timer.value < 0) {
      clearInterval(countdownTimer);
      timer.value = 0;
      timeOfMode.value = 0;
    }
  }, 1000);
};




const changeMode = (mode) => {
  if (mode === "easy") {
    timeOfMode.value = 20;
  }
  if (mode === "medium") {
    timeOfMode.value = 15;
  }
  if (mode === "hard") {
    timeOfMode.value = 10;
  }
  valueOfMode.value = mode.toUpperCase()
  timer.value = timeOfMode.value;
  progressWidth.value = "100%";
  startGame();
  startCountdownTimer();
};


const startGame = () => {
  gameInProgress.value = true;
  score.value = 0;
  modeGame.value = false;
  generateRandomColor();
};

const gameOverEnd = () => {
  score.value = 0;
  progressWidth.value = "0";
  modeGame.value = true;
  generateRandomColor();
};


const closeTheGame = () => {
  gameInProgress.value = false;
  playerPage.value = false;
  namePlayer.value = "";
  modeGame.value = false;
  score.value = 0;
  timer.value = 0;
};

const clickToNamePlayerPage = () => {
  playerPage.value = true;
};

const clickToSelectMode = () => {
  modeGame.value = true;
  gameInProgress.value = true;
};

</script>

<template>

  <div class="w-full h-screen bg-[url('../src/assets/FluentEmojiFlatPaintbrush.svg')] bg-white">
    <div class="bg-[#67274C] p-4 w-full h-[10%] ">
      <div class="flex items-center text-[#E9BCB9] space-x-2 justify-center">
        <span class="text-[#E9BCB9] text-4xl ml-5 font-extrabold">Click the color
          <span class="rounded-sm p-0.5 stroke text-red-600">NOT</span> the
          word</span>
        <TwemojiArtistPalette />
      </div>
    </div>
    <!-- tag side -->
    <div class="space-x-3 h-[80%] mx-[5%] my-[3%] flex ">
      <div class="w-full bg-gradient-to-l from-[#B296FF] to-[#C1D2DC]  rounded-xl   h-full ">
        <div class="flex bg-[#492154] rounded-t-xl justify-end h-[7%]">
          <div class="flex space-x-2 justify-end m-2 p-1">
            <span class="dot bg-[#8FFF5A]"></span>
            <span class="dot bg-[#FB6161]"></span>
            <span class="dot bg-[#FBFE6D]"></span>
          </div>
        </div>
        <div class="h-[93%]">
          <div class=" flex flex-col pt-20   space-y-10" v-if="!gameInProgress && !playerPage">
            <div class="space-x-2 flex justify-center">
              <span class="dot1 bg-purple-600"></span>
              <span class="dot1  bg-pink-400"></span>
              <span class="dot1 bg-[#8FFF5A]"></span>
              <span class="dot1 bg-[#FB6161]"></span>
              <span class="dot1 bg-[#FBFE6D]"></span>
              <span class="dot1 bg-blue-600"></span>
              <span class="dot1 bg-orange-400 "></span>
            </div>
            <button @click="clickToNamePlayerPage"
              class="  bg-[#312B5D] hover:bg-[#1C1743] text-white  w-8/12 text-center m-auto rounded-3xl text-8xl p-4 ">PLAY</button>

            <div class=" flex justify-center ">
              <label for="my_modal_6" class="btn">How to play ❔</label>
              <input type="checkbox" id="my_modal_6" class="modal-toggle" />
              <div class="modal">
                <div class="modal-box">
                  <h3 class="font-bold text-lg text-center">Instruction</h3>
                  <p class="py-4">
                  <div class="list-decimal">
                    <li>Once the game starts, the timer will begin counting down from a certain value (e.g., 5 seconds).
                    </li>
                    <li>Look at the words displayed on the screen. Each word is written in a specific color.
                    </li>
                    <li>Your goal is to click on the word that matches the color of the text, not the text itself. For
                      example:
                      - If the word "Red" is displayed in blue color, click on the word "Blue."
                      - If the word "Green" is displayed in green color, click on the word "Green."
                    </li>
                    <li>Continue making selections as quickly as possible within the time limit.</li>
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

          <div v-if="gameInProgress" class="relative ">
            <div class="flex justify-end text-red-600 hover:text-red-700 hover:scale-110 absolute top-3 right-3.5">
              <button @click="closeTheGame">
                <MaterialSymbolsCancel />
              </button>
            </div>
          </div>

          <!-- เขียนชื่อ -->
          <div v-if="playerPage && !gameInProgress" class=" m-auto flex  items-center justify-center h-full  ">

            <div>
              <div
                class=" flex flex-col items-center space-y-8 bg-gradient-to-r from-[#F9B57D] to-[#ECA9A5]   rounded-lg w-fit   p-20  ">
                <div class=" text-black text-4xl font-bold  flex space-x-2 items-center">Enter your name <TwemojiBackhandIndexPointingDownMediumDarkSkinTone/></div>
                <div class=" flex flex-col items-center space-y-1.5">
                  <span><input type="text" class="text-2xl rounded-md bg-white p-2 text-black" placeholder="Your name"
                      v-model.trim="namePlayer"></span>
                  <span class="text-red-500 font-bold" v-if="namePlayer.length === 0">**Please type your name**</span>
                </div>
                <div class=" flex space-x-3">
                  <button @click="closeTheGame"
                    class=" bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 border border-red-700 rounded ">Back</button>
                  <button @click="clickToSelectMode"
                    :class="namePlayer.length === 0 ? 'bg-white text-gray-300 font-bold py-2 px-4 border border-gray-500 rounded cursor-not-allowed' : 'bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 border border-green-700 rounded'"
                    :disabled="namePlayer.length === 0">Next</button>
                </div>
              </div>
            </div>
          </div>

          <div class="m-auto flex flex-col items-center justify-center space-y-3 p-20" v-if="modeGame">
            <h1 class="text-black text-4xl font-bold">Select Your Mode :</h1>
            <div class=" text-white flex flex-col pt-4 space-y-3.5 w-40 text-2xl">
              <button
                class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 border border-green-700 rounded"
                @click="changeMode('easy')">Easy</button>
              <button
                class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-2 px-4 border border-yellow-700 rounded"
                @click="changeMode('medium')">Medium</button>
              <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 border border-red-700 rounded"
                @click="changeMode('hard')">Hard</button>
            </div>
          </div>
          <div v-if="gameInProgress && !modeGame" class="pt-12">

            <div v-if="timer > 0">
              <div class="flex  justify-between text-2xl p-1 px-3 bg-white mx-20 rounded-2xl">
                <h1 class=" flex flex-row items-center text-[#1C1743]">
                  <TwemojiArtist />
                  <div>: <b>{{ namePlayer }}</b></div>
                </h1>
                <h1 class="  flex flex-row text-[#1C1743] items-center font-bold  "> Mode : <b>{{ valueOfMode }}</b></h1>
                <h1 class=" flex flex-row text-[#1C1743] items-center space-x-1 "><div >Score :</div> <b >{{ score }}</b></h1>
              </div>
              <div class="rounded-sm text-white font-semibold text-sm "
               :class="timer < 4 ? 'bg-red-500 h-5 mt-1.5' : 'progressBar'"
                :style="{ 'width': progressWidth, 'transition': 'width 1500ms linear', 'margin-top': '6px' }">
              </div>
              <div class=" strokeBack bg-white text-[170px] text-center p-0  mx-20  my-2 rounded-3xl"
                :style="{ color: data.randomColor }">
                <span> {{ data.randomColorName }} </span>
              </div>
              <div class="flex space-x-4 px-4 justify-center mt-9  rounded-lg">
                <div v-for="color in shuffledArray" :style="{ background: color }">
                  <button @click="checkColor(color)" class=" p-3 text-white font-bold text-3xl"
                    :style="{ 'background-color': color.toLowerCase() }">{{ color
                    }}</button>
                </div>
              </div>
            </div>
            <div class=" bg-white text-[50px] rounded-3xl text-center p-2 mt-[4%] mx-[20%] h-full text-black space-y-2" v-else >
              <h1 class=" text-red-500 font-extrabold text-[70px]">Game over !</h1>
              <div><b>{{ namePlayer }}</b>'s score : <b class=" rounded-lg bg-red-300 p-2">{{ score }}</b></div>
              <button class="rounded-xl p-2 buttonClick bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-2 px-4 border border-yellow-700 " @click="gameOverEnd">Try again ?</button>
            </div>
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

.progressBar {
  background-color: #492154;
  height: 20px;
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

.strokeBack {
  -webkit-text-stroke: 2px black;
}

.stroke {
  -webkit-text-stroke: 1.5px white;
}

.color2 {
  -webkit-text-stroke: 2px black;
}</style>
