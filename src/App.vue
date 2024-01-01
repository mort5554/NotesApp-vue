<script setup>
    import {ref} from "vue";
    import { useDateFormat, useNow, formatDate} from "@vueuse/core"

    const showModal = ref(false)

    const newNote = ref("")
    const notes = ref([])

    function getRandomColor(){
        return "hsl(" + Math.random() * 360 +", 100%, 75%)";
        return color
    }

    const addNote = () => {
        notes.value.push({
            id: Math.floor(Math.random() * 1000),
            text: newNote.value,
            date: ref(useDateFormat(useNow(), "H:m dddd D MMMM YYYY")),
            backgroundColor: getRandomColor()
        })
        showModal.value = false;
        newNote.value = ""
    }
//.toLoaleDateString("") 
</script> 

<template>
    <main>
        <div v-show="showModal" class="overlay">
            <div class="modal">
                <textarea v-model="newNote" name="note" id="note" cols="33" rows="13"></textarea>
                <button @click="addNote">Add Note</button>
                <button class="close" @click="showModal = !showModal">Close</button>
            </div>
        </div>
        <div class="container">
            <header>
                <h1>Notes</h1>
                <button @click="showModal = true">+</button>
            </header>
            <div class="card-container">
                <div v-for="note in notes" class="card" :style="{backgroundColor: note.backgroundColor}">
                     <p class="main-text">{{note.text}}</p>
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
        width: 350px;
        height: 350px;
        background-color: white;
        border-radius: 10px;
        padding: 30px;
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;
        margin-bottom: 10px;
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
        width: 200px;
        height: 200px;
        background-color: orange;
        padding: 10px;
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        margin-right: 20px;
        margin-bottom: 20px;
    }
    .date{
        font-size: 11px;
        font-weight: bold;
    }
    .main-text{
        text-wrap: wrap;
    }
    .card-container{
        width: 60vh;
        display: flex;
        flex-direction: row;
        text-wrap: wrap;
    }
</style>
