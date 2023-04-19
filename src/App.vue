<script setup>
  import {ref} from "vue"

  const showModal = ref(false)
  const newNote = ref("")
  const modalError = ref("")
  const noteLength = ref(180)
  const notes = ref([])

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = () => {
    if(newNote.value.length < 10 || newNote.value.length > noteLength.value) {
      if(newNote.value.length < 10) {
        modalError.value = "Please enter more than 10 characters"
        return
      } else if(newNote.value.length > noteLength.value) {
        modalError.value = `Note be ${noteLength.value} characters or less.`
        return
      } else {
        modalError.value = "Unknown Error. Refresh the page and try again."
        return
      }
    } else {
      notes.value.push({
      id: Math.floor(Math.random() * 10000000),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomColor()
    })
    showModal.value = false
    modalError.value = ""
    newNote.value = ""
    }

  }
  
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <header>
          <h2 class="modal-header">Notetaker</h2>
          <button @click="showModal = false" class="modal-close-button">X</button>
        </header>
        <p class="modal-subheader">Write your note, ensure that it is under {{ noteLength }} characters</p>
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <div class="modal-data">
          <p class="modal-error">{{ modalError }}</p>
          <p class="character-count">{{ newNote.length }}/{{ noteLength }}</p>
        </div>
        
        <button @click="addNote" class="modal-button">Add Note</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true" class="header-button">+</button>
      </header>
      <div class="cards-container">
        <div 
        v-for="note in notes" 
        :key="note.id"
        class="card" 
        :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-GB") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>


<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }

  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }

  .header-button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: #2c3e50;
    color: white;
    font-size: 32px;
    text-align: center;
    line-height: 0%;
    border-radius: 100%;
  }

  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .date {
    font-size: 12.5px;
    font-weight: bold;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal-button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: #2c3e50;
    border: none;
    color: white;
    cursor: pointer;
    border-radius: 6px;
    margin-top: 5px;
  }

  .modal-close-button {
    border: none;
    width: 30px;
    height: 30px;
    cursor: pointer;
    background-color: #2c3e50;
    color: white;
    font-size: 14px;
    text-align: center;
    line-height: 0%;
    border-radius: 6px;
  }

  .modal-header {
    margin-top: -15px;
  }

  .modal-subheader {
    margin-top: -10px;
    margin-bottom: 15px;
  }

  .modal-data {
    display: flex;
    justify-content: space-between;
  }

  .modal-error {
    color: red
  }

  .character-count {
    text-align: right;
  }
</style>