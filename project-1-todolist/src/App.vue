<script setup>
import { ref, reactive } from "vue"

import IconParkSolidCorrect from "../src/assets/IconParkSolidCorrect.vue"
import MaterialSymbolsAdd from "../src/assets/MaterialSymbolsAdd.vue"
import MaterialSymbolsDeleteOutline from "../src/assets/MaterialSymbolsDeleteOutline.vue"
//add note
const tasks = reactive({
  notes: [] ,
  newNote: '',
  newDescription:''
})

console.log(tasks.notes);
//pop-up
 
const add = () => {
  // const note = {title: noteTitle, description: noteDescription}
  if (tasks.newNote.trim() !== '' || tasks.newDescription.trim() !== ''){
    tasks.notes.push({
    Title: tasks.newNote, 
    Description: tasks.newDescription
    })
  }
}



const addNote = ref(false)
const toggleAddnote = () => {
  addNote.value = !addNote.value
}
//filter tag
//delete note
//done note and move to category complete
</script>

<template>
  <div class="w-full h-screen pb-3">
    <div class="bg-[#1D1A39] p-4 absolute w-full">
      <div class="flex items-center text-white space-x-2">
        <span class="text-white text-4xl ml-5 font-extrabold">TO DO LIST </span>
        <IconParkSolidCorrect />
      </div>
    </div>
    <!-- tag side -->
    <div class="flex space-x-3 pr-4 pt-24 h-full pb-1">
      <div
        class="flex flex-col w-1/6 bg-[#E99B58] ml-5 rounded-xl p-3 font-bold">
        <button class="bg-[#1D1A38] text-white">All</button>
        <button class=" ">Work/School</button>
        <button class=" ">Routine</button>
        <button class=" ">Completed</button>
      </div>

      <div class="w-5/6 bg-[#312C58] rounded-xl">
        <div class="flex bg-[#1D1A39] rounded-t-xl justify-end">
          <div class="flex space-x-2 justify-end m-2 p-1">
            <span class="dot bg-[#8FFF5A]"></span>
            <span class="dot bg-[#FB6161]"></span>
            <span class="dot bg-[#FBFE6D]"></span>
          </div>
        </div>
        <div v-for=" (note, index) in tasks.notes" :key="index"
        class=" text-white ">
            {{  note.Title }}
            {{  note.Description }}
        </div>

        <!-- v-show to add note if user click add note-->

        <!-- <div class="relative flex flex-col" v-show="addNote = false">


            <button class="bottom-0 right-5 absolute w-12 h-12 rounded-full inline-flex items-center"
              @click="toggleAddnote()">
              <MaterialSymbolsAdd />
            </button>

          </div> -->
        <div class="relative flex flex-col p-3" v-show="addNote">
          <div class="grid justify-items-center pt-3">
           <!-- showing post it when user click add note -->
            <form @submit.prevent="add">
            <div class="postit p-6">
              <h1>Title</h1>
              <input class="border-2 bg-gray-300"  v-model="tasks.newNote"/>
              <h1>Description</h1>
              <input class="border-2 bg-gray-300" v-model="tasks.newDescription"/>
              <button type="submit" >Add</button>
            </div>
          </form>
          </div>
        </div>
        <button type="submit"
          class="bottom-4 right-5 absolute w-12 h-12 rounded-full inline-flex items-center"
          @click="toggleAddnote()">
          <MaterialSymbolsAdd />
        </button>
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
</style>
