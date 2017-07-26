<template>
    <div id="app" class="container">
    <div class="page-header">
      <h1>Vue Js 2 and Firebase application</h1>   
      </div>  
        <div class="panel panel-default">
        <div class="panel-heading">  <h3>Add book</h3></div>
        <div class="panel-body">
            <form id="form" class="form-inline" v-on:submit.prevent="addBook">
                <div class="form-group">
                    <label for="bookTitle">Title:</label>
                    <input type="text" name="bookTitle" class="form-control" v-model="newBook.title">
                </div>
                    <div class="form-group">
                        <label for="bookAuthor">Author:</label>
                        <input type="text" name="bookAuthor" class="form-control" v-model="newBook.author">
                    </div>
                        <div class="form-group">
                         <label for="bookUrl">URL:</label>
                         <input type="text" name="bookUrl" class="form-control" v-model="newBook.url">
                        </div>
                        <input type="submit" name="" class="btn btn-primary" value="Add  Book">

            </form>
        </div>
          
        </div>
      <div class="panel panel-default">
        <div class="panel-heading">
        <h3>Book Lists</h3>
        </div>
        <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
            </tr>
          </thead>
          <tr v-for="book in books">
            <td><a v-bind:href="book.url">{{book.title}}</a></td>
            <td>{{book.author}}</td>
            <td><span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span></td>
          </tr>
        </table>
        </div>
      </div>
    </div>
</template>

<script>
import Hello from './components/Hello'
import Firebase from 'firebase'
import toastr from 'toastr'

let config ={
    apiKey: "AIzaSyD6_vkLv_J1Vx3JRlmCf9LDCYKK483w3sI",
    authDomain: "vue-app-d72d1.firebaseapp.com",
    databaseURL: "https://vue-app-d72d1.firebaseio.com",
    projectId: "vue-app-d72d1",
    storageBucket: "vue-app-d72d1.appspot.com",
    messagingSenderId: "502247632844"
}

 let app = Firebase.initializeApp(config);
 let db = app.database();

 let booksRef = db.ref('books');


export default {
  name: 'app',
  firebase:{
     books: booksRef
  },
  data(){
    return {
      newBook:{
        title: '',
        author: '',
        url:''
      }
    }
  },
  methods:{
    addBook: function(){
      booksRef.push(this.newBook)
      this.newBook.title= '';
      this.newBook.author='';
      this.newBook.url='';
    },
    removeBook: function(book){
      booksRef.child(book['.key']).remove();
      toastr.success("Book Removed");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
