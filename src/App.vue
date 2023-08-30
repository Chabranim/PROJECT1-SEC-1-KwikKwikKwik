<script setup>
import { reactive } from "vue"
import { onMounted } from 'vue'


import IconParkSolidCorrect from "../src/assets/IconParkSolidCorrect.vue"
import PepiconsPopPaintPallet from "../src/assets/PepiconsPopPaintPallet.vue"

const data = reactive({
  words: ["Red", "Blue", "Green", "Yellow", "Gray", "Orange", "Black"],
  colors: ["black", "blue", "pink", "purple", "orange", "yellow"],
  randomColorName: '',
  randomColor: ''
})

const getRandomIndex = (max) => {
  return Math.floor(Math.random() * max);
}


const generateRandomColor = () => {
  data.randomColorName = data.words[getRandomIndex(data.words.length)];
  data.randomColor = data.colors[getRandomIndex(data.colors.length)];

}
const randomTrue = () => {
  data.randomColorName = generateRandomColor()
  data.randomColor = generateRandomColor()
}
const checkColor = (color) => {
  let currentColor= data.randomColor;
  // randomTrue()
}

// const generateRandomColor = () => {
//       const randomIndex = Math.floor(Math.random() * this.colors.length);
//       this.randomColor = this.colors[randomIndex];
//       this.randomColorName = this.colorNames[randomIndex];
//     }

// const checkColor = () => {
//   // Compare clicked color with current color
//   if (data.currentWord.toLowerCase() === data.currentColor) {
//     alert("Correct! Well done!");
//   } else {
//     alert("Oops! Try again.");
//   }
//   this.updateStroopTest();
// }
onMounted(() => {
  generateRandomColor();
  // randomTrue()
})

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

        <div class=" color " :style="{'background-color': 'white',color: data.randomColor}">{{ data.randomColorName }}</div>
        <div v-for="color in data.words">
          <button @click="checkColor(color)">{{ color }}</button>
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
  font-size: 120px;
  text-align: center;
  padding: 20px;
  margin: 5%;
  border-radius: 25px;
  -webkit-text-stroke: 2px black;
}
.stroke {
  -webkit-text-stroke: 1.5px white;
}
</style>
