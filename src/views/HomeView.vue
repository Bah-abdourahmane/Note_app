<script setup>
import { ref } from 'vue';
const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

function generateRandomColorHsl() {
  const hue = Math.floor(Math.random() * 360);
  const saturation = Math.floor(Math.random() * (100 + 1)) + "%";
  const lightness = Math.floor(Math.random() * (100 + 1)) + "%";
  return "hsl(" + hue + ", " + saturation + ", " + lightness + ")";
}
const addNote = ()=>{
  if(newNote.value.length <10){
    return errorMessage.value = "Note needs to be 10 characters or more"
  }
  notes.value.push({
    id : Math.floor(Math.random() * 1000000),
    text : newNote.value.trim(),
    date : new Date().toLocaleDateString("en-US"),
    bg : generateRandomColorHsl(),
  });
  showModal.value = false;
  newNote.value = "";
}
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea name="note" id="note" v-model="newNote" cols="30" rows="5" placeholder="Write your note here...."></textarea>
        <span  class="error">{{ errorMessage }}</span>
        <button class="addbtn" @click="addNote()">Add Note</button>
        <button class="closebtn" @click="showModal = !showModal">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button class="modalopenbtn" @click="showModal = !showModal">+</button>
      </header>
      <div class="cards-container">
        <div class="card" v-for="(note,i) in notes" :key="i" :style="{background: note.bg}">
          <p class="main-text">{{note.text}}</p>
          <p class="date">{{note.date}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
.container{
  max-width: 1100px;
  padding: 10px;
  margin: 0 auto;
}
header{
  position: fixed;
  top: 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 88%;
  background: #fff;
  z-index: 11;
}
h1{
  font-weight: bold;
  font-size: 45px;
}
.modalopenbtn{
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  cursor: pointer;
  color: white;
  font-size: 20px;
  background: rgba(0,0,0,0.5);
}
.cards-container{
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  justify-content: start;
  gap: 10px;
  margin-top: 120px;
}
.card{
  width: 225px;
  height: 225px;
  background: rgb(237,182,44);
  padding: 5px 10px;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  overflow: hidden;
  color: grey;
}
.main-text{
  max-height: 190px;
  height: 100%;
  overflow: hidden;
  letter-spacing: 2px;
  font-size: 18px;
}
.date{
  font-weight: bold;
  font-size: 16px;
}
.overlay{
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0,0,0,0.3);
  z-index: 13;
}
.modal{
  width: 400px;
  display: flex;
  flex-direction: column;
  background: #fff;
  padding: 20px;
  border-radius: 12px;
}
.addbtn, .closebtn{
  width: 100%;
  font-size: 20px;
  color: #fff;
  padding: 10px 20px;
  background: blueviolet;
  border: none;
  cursor: pointer;
  margin-top: 10px;
}
.closebtn{
  margin-top: 5;
  background: tomato;
}
#note{
  resize: none;
  outline: blueviolet;
  padding: 10px;
  font-size: 16px;
}
.error{
  color: tomato;
  font-weight: bold;
  margin-top: 5px;
}
</style>