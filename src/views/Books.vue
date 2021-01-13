<template>
  <div class="books">
    <br>
    <h1>{{ message}}</h1>
    <br>
    <br>
    <br>
    <form v-on:submit.prevent="submit()">
      <h1>Make a new book</h1>
      <div class="form-group">
        <label>Title:</label> 
        <input type="text" class="form-control" v-model="name">
      </div>
      <div class="form-group">
        <label>Author:</label>
        <input type="text" class="form-control" v-model="author">
      </div>
      <input type="submit" class="btn btn-primary" value="Submit">
    </form>
    <br>
    <br>
    <br>
    <div v-for="book in books">
      {{"Title: " + book.name }}
      <br>
      {{ "Author: " + book.author }}
      <br>
      <br>
     <button v-on:click="showBook(book)">Add Book To Your Profile</button>
     <br>
     <br>
     <br>
    </div>
    <dialog id="book-status">
      <form method="dialog">
        <h1>Book info</h1>
        <p><select v-model="status" class="form-select" aria-label="Default select example">
        <option selected>Select Your Reading Option</option>
        <option value="Want to read">Want To Read</option>
        <option value="Currently Reading">Currently Reading</option>
        <option value="Read">Read</option>
        </select></p>
        <p><select v-model="rating" class="form-select" aria-label="Default select example">
        <option selected>Rating</option>
        <option value="1">One Star</option>
        <option value="2">Two Stars</option>
        <option value="3">Three Stars</option>
        <option value="4">Four Stars</option>
        <option value="5">Five Stars</option>
        </select></p>
        <button v-on:click="booksCreate()">Create Book</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Book Log",
      books: [],
      currentBook: {},
      status: "",
      rating: "",
      author: "",
      name: "",
    };
  },
  created: function () {
    this.booksIndex();
  },
  methods: {
    booksIndex: function () {
      console.log("books index");
      axios.get("/api/books").then((response) => {
        console.log(response.data);
        this.books = response.data;
      });
    },
    showBook: function (book) {
      console.log("showing book");
      this.currentBook = book;
      document.querySelector("#book-status").showModal();
    },
    submit: function () {
      var params = {
        name: this.name,
        author: this.author,
      };
      axios.post("/api/books", params).then((response) => {
        this.$router.push(response.data);
      });
    },
    booksCreate: function () {
      console.log("adding book");
      var params = {
        book_id: this.currentBook.id,
        status: this.status,
        rating: this.rating,
      };
      axios.post("/api/book_users", params).then((response) => {
        console.log(response.data);
        this.$router.push(response.data);
      });
    },
  },
};
</script>

