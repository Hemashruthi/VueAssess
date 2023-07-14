<template>
    <img src="/public/image.jpg" class="book">
    <h1>Hello {{ username }}</h1>
    <h1>Welcome to Library System</h1>
<div>
  <input type="text" class="search"  v-model="searchTerm" />
  <button  @click="searchBook" class="searchButton">Search</button>
</div>
<p v-if="noResultsMessage" class="no-results">{{ noResultsMessage }}</p>
  <table>
    <thead>
      <tr>
        <th>Book ID</th>
        <th>Book Title</th>
        <th>Author</th>
        <th>Genre</th>
        <th>Published</th>
        <th>Edit</th>
        <th>Delete</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="book in books" :key="book.id">
        <td>{{ book.book_id }}</td>
        <td>{{ book.book_title }}</td>
        <td>{{ book.author }}</td>
        <td>{{ book.genre }}</td>
        <td>{{ book.published }}</td>
        <td>
            <button @click="editBooks(book)" class="edit">Edit</button>
        </td>
        <td>
            <button @click="deleteBook(book.id)" class="del">Delete</button>
        </td>
      </tr>
    </tbody>
  </table><br><br>
  <FormSup @book-added="addBook"></FormSup>
</template>

<script>
import axios from "axios";
import FormSup from "./FormSup.vue";


export default {
  components: { FormSup },
  data() {
    return {
      books: [],
      editbook : null,
    //   priceRange:''
    };
  },
  created (){
    this.emitter.on('add', (evt) => {
      this.editbook = evt.eventContent;
      
      console.log(this.editbook)
      this.addBook(this.editbook);
    });

    
  },
  mounted() {
    this.username = localStorage.getItem('username');
    this.fetchBooks();
  },
  methods: {
    fetchBooks() {
      axios
        .get("http://127.0.0.1:3333/display")
        .then((response) => {
          this.books = response.data;
        })
        .catch((error) => {
          console.error("Error fetching books:", error);
        });
    },resetForm() {
      this.editbook = {
        book_id: '',
        book_title: '',
        author: '',
        genre: '',
        published: '',
      };
    },
     addBook(newBook) {
        
      console.log(newBook,"new")
      axios
        .post("http://127.0.0.1:3333/insert", this.editbook)
        .then((response) => {
          this.books.push(response.data);
          this.resetForm();
        })
        console.log(this.editbook)
        .catch((error) => {
          console.err("Error adding book:", error);
        });
    },
    deleteBook(id) {
    axios
    .delete(`http://127.0.0.1:3333/delete/${id}`)
    .then((response) => {
      console.log('Data deleted successfully:', response.data);
      this.fetchBooks();
    })
    .catch((error) => {
      console.error('Error deleting data:', error);
    });
    },

    searchBook() {
  axios
    .get('http://127.0.0.1:3333/searchBook', {
      params: {
        term: this.searchTerm,
      },
    })
    .then((response) => {
      console.log(response.data);
      if (response.data.length === 0) {
        this.books = []; 
        this.noResultsMessage = 'No matching books found.';
      } else {
        this.books = response.data;
        this.noResultsMessage = '';
      }
    })
    .catch((error) => {
      console.error('Error searching books:', error);
    });
},

  editBooks(book) {
//console.log("vjh")
    this.emitter.emit('getId', {'eventContent': book.id})
  },
  }
};
</script>

<style scoped>
        table {
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td{
            width: 50%;
            border: 1px solid black;
            padding: 15px;
            text-align: center;
        }
        th {
            font-weight: 700;
            background-color: rgba(0, 0, 0, 0.153);
        }
        .edit {
            background-color: rgba(255, 217, 0, 0.637);
            color: black; 
            padding: 5px 10px; 
            border: none; 
            border-radius: 4px;
            cursor: pointer;
        }
        .del {
            background-color: rgba(255, 0, 0, 0.715);
            color: black; 
            padding: 5px 10px; 
            border: none; 
            border-radius: 4px;
            cursor: pointer;
        }
        .search{
           margin-top: 20px;
  border: 1px solid #060506;
  border-right: none;
  padding: 10px;
  height: 25px;
  /* border-radius: 5px 0 0 5px; */
  outline: none;
  color: #000000;
        }
      

        .searchButton {
            margin-top: 20px;
  height: 25px;
  border: 1px solid #18191a;
  background: #933ba4;
  text-align: center;
  color: #fff;
  /* border-radius: 0 5px 5px 0; */
  cursor: pointer;

}
h1 {
    font-weight: 800;
}
.book {
    width: 100%;
    margin-top: 0px;
    height: 200px;
}
</style>