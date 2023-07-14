<template>
    <h2>Would you like to add any book?</h2>
    <button @click="showForm = true" class="add">Add Book</button>
    <form v-if="showForm" @submit.prevent="addBook">
        
      <label>Book Id(5-digit code)</label>
      <input type="text" id="bookId" v-model="newBook.book_id" required pattern="[0-9]+" minlength="5" maxlength="5">
      
      <label>Book Title:</label>
      <input type="text" id="bookTitle" v-model="newBook.book_title" required pattern="^[a-zA-Z\s]+$">
      
      <label>Author</label>
      <input type="text" id="author" v-model="newBook.author" required pattern="^[a-zA-Z\s]+$">
      
      <label>Genre</label>
      <input type="text" id="genre" v-model="newBook.genre" required pattern="^[a-zA-Z\s]+$">

      <label>Published</label>
      <input type="text" id="published" v-model="newBook.published" required pattern="[0-9]+" maxlength="4" minlength="4">
      
      <button class="add" type="submit">{{this.edit ? 'update' : 'create'}}</button>
    </form>

</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      showForm: false,
      edit:false,
      newBook: {
        book_id: '',
        book_title: '',
        author: '',
        genre:'',
        published: ''
      },
      id: '',
    };
},
created (){
    this.emitter.on('getId', (evt) => {
      this.id = evt.eventContent;
      this.showForm = true
      console.log(this.id)
      this.fetchBook();
    })
  },
  methods: {
    addBook() {
      if(this.edit){ 
        this.editItem();
       }
      else{
        
        this.emitter.emit('add', {'eventContent': this.newBook})
      }
     
      this.resetForm();
    },
    resetForm() {
      this.newBook = {
        book_id: '',
        book_title: '',
        author: '',
        genre: '',
        published: ''
        
      };
      this.showForm = false;
      this.edit=false;
    },
    editItem(){
    axios
          .put(`http://127.0.0.1:3333/update/${this.id}`, this.newBook)
          .then((response) => {
            console.log(response);
            this.fetchBooks();
          })
          .catch((error) => console.error(error));
          
         location.reload();
  },fetchBook() {
        axios
          .get(`http://127.0.0.1:3333/selectById/${this.id}`)
          .then((response) => {
            this.edit = true;
            this.newBook = response.data;
            console.log(response.data)
          })
          .catch((error) => console.error(error));
      },
  }
};

</script>

<style scoped>
form {
      max-width: 1890px;
      margin-top: 0; 
      background: rgba(255, 192, 203, 0.603);
      text-align: left;
      padding: 90px;
      border-radius: 10px;
    }
    label {
      color: #252424;
      display: inline-block;
      margin: 15px 0 5px;
      font-size: 0.6em;
      text-transform: uppercase;
      letter-spacing: 1px;
      font-weight: bold;
    }
    input, select {
      display: block;
      padding: 10px 6px;
      width: 100%;
      box-sizing: border-box;
      border: none;
      border-bottom: 1px solid #ddd;
      color: #555;
    }
    input[type="radio"] {
      display: inline-block;
      width: 16px;
      margin: 0 10px 0 0;
      position: relative;
      top: 2px;
    }
    input[type="checkbox"] {
      display: inline-block;
      width: 16px;
      margin: 0 10px 0 0;
      position: relative;
      top: 2px;
    }
    button {
      background: #046b10;
      border: 0;
      padding: 10px 20px;
      margin-top: 20px;
      color: white;
      border-radius: 20px;
    }
    .submit {
      text-align: center;
    }
    img {
      max-width:5%;
    }
   
    .add{
        background-color: #046b10;
            color: white; 
            padding: 10px 10px; 
            border: none; 
            border-radius: 4px;
            cursor: pointer;
    }
    .h2{
        font-weight: 600;
    }
</style>