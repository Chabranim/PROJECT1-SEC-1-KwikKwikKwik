<script setup>
import { reactive, ref } from "vue"
import { onMounted } from 'vue'


import IconParkSolidCorrect from "../src/assets/IconParkSolidCorrect.vue"
import PepiconsPopPaintPallet from "../src/assets/PepiconsPopPaintPallet.vue"

const data = reactive({
  words: ["Red", "Blue", "Green", "Yellow", "Pink", "Orange", "Black", "Purple"],
  colors: ["black", "blue", "pink", "purple", "orange", "yellow","red"],
  randomColorName: '',
  randomColor: ''
})

let score = ref(0)


const getRandomIndex = (max) => {
  return Math.floor(Math.random() * max);
}


const generateRandomColor = () => {

  data.randomColorName = data.words[getRandomIndex(data.words.length)];
  data.randomColor = data.colors[getRandomIndex(data.colors.length)];
  // console.log(data.randomColorName);
  // console.log(data.randomColor);
}

let correct = ref()
const checkColor = (color) => {
  console.log(color);
  if (color.toLowerCase() === data.randomColor) {
    correct.value = true
    
    // console.log(correct.value);
    score.value++
  } else {
    correct.value = false

    // console.log(correct.value);
    if(score < 0){
      score.value --
    }
  }
  generateRandomColor()
}
// console.log(checkColor());




onMounted(() => {
  generateRandomColor();
  // setInterval(generateRandomColor, 3500)
  // checkColor()  
  // countdownTime()
  
})

let timer = ref('15');
const countdownTime = setInterval(() => {
  if (timer.value === 0) {
    clearInterval(countdownTime)
    // alert(`Your total score : ${score.value}`)                
  } else {
    timer.value--
    // console.log(timer.value)  
  }             
}, 1000)
// setTimeout(alert(`total score: ${score.value}`), 20000)
</script>

<template>
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
        <h1 class=" bg-white">Score: {{ score }}</h1>
        <h1 class=" bg-white">Timer : {{ timer }}</h1>
        <!-- <h1 v-show="correct == true"> Great !</h1> -->
        <div class=" color2 bg-white" v-show="timer === 0">
                     Game over ! your score : {{ score }}
          <button class=" border border-black">Try again ?</button>
        </div>
        <div v-show="timer > 0" >
          <div  class=" color" :style="{'background-color': 'white',color: data.randomColor}">{{ data.randomColorName }}</div>
          <div v-for="color in data.words">
            <button @click="checkColor(color)" >{{ color }}</button>
          </div>
        </div>
        <!-- <div>
          <button @click="checkColor('red')" >Red</button>
          <button @click="checkColor(value)">Blue</button>
          <button @click="checkColor(value)" >Green</button>
          <button @click="checkColor(value)" >Yellow</button>
          <button @click="checkColor(value)" >Gray</button>
          <button @click="checkColor(value)" >Orange</button>
          <button @click="checkColor(value)" >Black</button>
        </div> -->
        

      </div>
    </div>
  </div>
</template>

<style>
@font-face {
  font-family: "dbheavent";
  src: url("./fonts/DBHeaventBd/DBHeavent.ttf") format("truetype");
}

button {
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

.postit {
  height: 30rem;
  width: 30rem;
  border-radius: 1em;
  background-color: white;
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
  margin-left: 25%;
  margin-right: 25%;
  border-radius: 25px;
  -webkit-text-stroke: 2px black;
}
</style>
