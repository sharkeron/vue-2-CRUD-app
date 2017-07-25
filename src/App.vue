<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 &amp; Firebase Sample Application</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>
          Add Book
        </h3>
      </div>
      <div class="panel-body">
        <form id="form" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="bookTitle">Title:</label>
            <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
          </div>

          <div class="form-group">
            <label for="bookAuthor">Author:</label>
            <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
          </div>

          <div class="form-group">
            <label for="bookUrl">Url:</label>
            <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
          </div>
          <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books Lists</h3>
      </div>

      <div class="panel-body">
        <table class="table table-striped">
          <thead><tr><th>Title</th>
          <th>Author</th>
          </tr></thead>
          <tbody>
            <tr v-for="book in books">
              <td>
                <a v-bind:href="book.url">{{book.title}}</a>
              </td>
              <td>{{book.author}}</td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

import Firebase from 'firebase'
import toastr from 'toastr'

toastr.options = {
    "progressBar": true
}

let config = {
  apiKey: "AIzaSyDsS6vQY22p6HkDGK7MFJf9rs8_STe_-3s",
  authDomain: "crud-application-vuejs-2.firebaseapp.com",
  databaseURL: "https://crud-application-vuejs-2.firebaseio.com",
  projectId: "crud-application-vuejs-2",
  storageBucket: "crud-application-vuejs-2.appspot.com",
  messagingSenderId: "639905395707"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let bookRef = db.ref('books');

export default {
  name: 'app',
  firebase: {
    books: bookRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function(){

      var titleId = document.getElementById('bookTitle');
      var authorId = document.getElementById('bookTitle');
      var urlId = document.getElementById('bookTitle');

      if(titleId.value !== ''){
        titleId.parentNode.classList.remove('has-error');
      }

      if(authorId.value !== ''){
        authorId.parentNode.classList.remove('has-error');
      }

      if(urlId.value !== ''){
        urlId.parentNode.classList.remove('has-error');
      }

      if(this.newBook.title !== '' &&
         this.newBook.author !== '' &&
         this.newBook.url !== ''){

            bookRef.push(this.newBook);
            this.newBook.title = '';
            this.newBook.author = '';
            this.newBook.url = '';
            toastr.success('Book added! =)');

     } else {

       if(titleId.value === ''){
          titleId.parentNode.classList.add('has-error');
       }

        if(authorId.value === ''){
          authorId.parentNode.classList.add('has-error');
       }

        if(urlId.value === ''){
          urlId.parentNode.classList.add('has-error');
       }

        toastr.error('Some fields is empty! =(');
      }
    },
    removeBook: function(book) {
      bookRef.child(book['.key']).remove();
      toastr.success('Book removed');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
