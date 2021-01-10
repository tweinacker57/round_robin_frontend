<template>
  <div class="books">
    <h1>{{ message}}</h1>
    
    <div v-for="book in books">
      {{ book.name }}
      <br>
      {{ "-" + book.author }}
      <br>
     <button v-on:click="showBook(book)">Add Book To Your Profile</button>

    </div>
    <dialog id="book-status">
      <form method="dialog">
        <h1>Book info</h1>
        <p><select class="form-select" aria-label="Default select example">
        <option selected>Select Your Reading Option</option>
        <option value="Want to read">Want To Read</option>
        <option value="Currently Reading">Currently Reading</option>
        <option value="Read">Read</option>
        </select></p>
        <p><select class="form-select" aria-label="Default select example">
        <option selected>Rating</option>
        <option value="1">One Star</option>
        <option value="2">Two Stars</option>
        <option value="3">Three Stars</option>
        <option value="4">Four Stars</option>
        <option value="5">Five Stars</option>
</select></p>
        <button v-on:click="booksCreate()">Submit</button>
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
      message: "Book Finder",
      books: [],
      currentBook: {},
      status: "",
      rating: "",
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

