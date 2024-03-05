<script setup>
    import {ref} from "vue";
    import { useDateFormat, useNow, formatDate} from "@vueuse/core"
    import  {useToast} from "vue-toastification"

    const toast = useToast()
   
//Local storage
  const onMounted = () => {
  const myLocalStorage = JSON.parse(localStorage.getItem
  ("notes"))

  if(myLocalStorage){
    notes.value = myLocalStorage
  }
}

    const showModal = ref(false)
    const showReWriteModal = ref(false)

    const newNote = ref("")
    const errorMessage = ref("")
    const notes = ref([])
    const reWriteText = ref("")

    function getRandomColor(){
        return "hsl(" + Math.random() * 360 +", 100%, 75%)";
    }

    const addNote = () => {
        if(newNote.value.length < 10){
            return errorMessage.value = "Note needs to be 10 characters or more"
        }
            
        notes.value.push({
            id: Math.floor(Math.random() * 1000),
            text: newNote.value,
            date: useDateFormat(new Date(), "H:m dddd D MMMM YYYY"),
            backgroundColor: getRandomColor(),
        })
        showModal.value = false;
        newNote.value = ""
        errorMessage.value == String

        toast.success("Note Added")

        saveMyLocalStorage()
    }
    const reWriteNote = (newText, id) => {
        console.log(newText)
        console.log(index)
        console.log(reWriteText.value)
        return reWriteText.value = ref(newText)
        //console.log(notes.value[id])
       // return newText = text
    }
    const changeText = (id) => {
        if(reWriteText.value.length < 10){
            return errorMessage.value = "Note needs to be 10 characters or more"
        }
            
        notes.value.push({
            id: id,
            text: reWriteText.value,
            date: useDateFormat(new Date(), "H:m dddd D MMMM YYYY"),
            backgroundColor: getRandomColor(),
        })
        showReWriteModal.value = false;
        reWriteText.value = ""
        errorMessage.value == String

        toast.success("Note changed")

        saveMyLocalStorage()
    }

    const deleteNote = (id) => {
        console.log("get id:",{id})
        //notes.value.splice(id)
        toast.error("Note deleted")
        
        notes.value = notes.value.filter(
        (note) => note.id !== id)
        
        saveMyLocalStorage()
    }
    
//Save to local storage
    const saveMyLocalStorage = () => {
        localStorage.setItem("notes", JSON.stringify(notes.value))
    }

    onMounted()
</script> 

<template>
    <main>
        <div v-show="showModal" class="overlay">
            <div class="modal">
                <textarea v-model.trim="newNote" name="note" id="note" cols="33" rows="13"></textarea>
                <p v-if="errorMessage">{{ errorMessage }}</p>
                <button @click="addNote">Add Note</button>
                <button class="close" @click="showModal = !showModal">Close</button>
            </div>
        </div>
        <div v-show="showReWriteModal" class="overlay">
            <div class="modal">
                <textarea v-model.trim="reWriteText.value" name="note" id="note" cols="33" rows="13"></textarea>
                <p v-if="errorMessage">{{ errorMessage }}</p>
                <button @click="changeText()">Change text</button>
                <button class="close" @click="showReWriteModal = !showReWriteModal">Close</button>
            </div>
        </div>-
        <div class="container">
            <header>
                <h1>Notes</h1>
                <button @click="showModal = true">+</button>
            </header>
            <div class="card-container">
                <div  
                    v-for="note in notes"
                    :key="note.id"
                    class="card" 
                    :style="{backgroundColor: note.backgroundColor}">
                     <img @click="deleteNote(note.id)" src="./components/icons/trashcan-icon2.png" class="trash">
                     <p @click="showReWriteModal = !showReWriteModal, reWriteNote(note.text, note.id)" 
                     class="main-text">{{note.text}}</p>
                     <p class="date">{{ note.date}}</p>
                </div>
            </div>
        </div>
    </main>
</template>
 
<style scoped>
html{
    padding: 0px;
    margin: 0px;
}
body{
    padding: 0px;
    margin: 0px;
}
    main{
        display: flex;
        justify-content: center;
        align-content: center;
        margin: auto;
        height: 100vh;
        width: 100vw;
        background-color: aliceblue;
    }
    .modal{
        width: 380px;
        height: 380px;
        background-color: white;
        border-radius: 10px;
        padding: 30px;
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        margin-bottom: 10px;
        animation: slideFromDown 0.5s ease-in-out;
    }
    @keyframes slideFromDown{
        from{
            transform: translateY(110%);
        }
        to{
            transform: translateY(0%);
        }
    }
    .overlay{
        position: absolute;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.1);
        z-index: 10;
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        align-content: center;
    }
    .modal button{
        padding: 10px 20px;
        font-size: 20px;
        width: 100%;
        background-color: blueviolet;
        border: none;
        color: white;
        cursor: pointer;
        margin-top: 20px
    }
    .modal .close{
        background-color: red;
        margin-top: 7px;

    }
    .modal p{
        color: red;
    }
    .container{
        max-width: 100vw;
        padding: 10px;
        margin: 0 auto;
    }
    header{
        display: flex;
        width: 90vw;
        justify-content: space-between;
        align-items: center;
    }
    header > h1{
        font-weight: bold;
        margin-bottom: 25px;
        font-size: 60px;
        display: flex;
    }
    header > button{
        border: none;
        padding: 10px;
        width: 40px;
        height: 40px;
        cursor: pointer;
        background-color: rgb(21, 20, 20);
        border-radius: 100%;
        color: white;
        font-size: 20px;
        align-self: center;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .card{
        min-width: 200px;
        min-height: 200px;
        background-color: orange;
        padding: 10px;
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        margin-right: 20px;
        margin-bottom: 20px;
    }
    .trash{
        display: flex;
        align-self: flex-end;
        padding: 8px;
        padding-right: 5px;
        height: 30px;
        width: 30px;
    }
    .date{
        font-size: 11px;
        font-weight: bold;
    }
    .main-text{
        word-wrap: break-word;
        padding: 4px;
        width: 170px;
        height: 180px;
    }
    .card-container{
        width: 100vh;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
    }
</style>
