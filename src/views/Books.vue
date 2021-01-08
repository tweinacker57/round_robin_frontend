<template>
  <div class="books">
    <h1>{{ message}}</h1>
    
    <div v-for="book in books">
      {{ book.name }}
     <button v-on:click="showBook(book)">Add Book To Your Profile</button>

    </div>
    <dialog id="book-status">
      <form method="dialog">
        <h1>Book info</h1>
        <p>Reading Status: <input v-model="status" type="text"></p>
        <p>Rating: <input v-model="rating" type="text"></p>
        <button v-on:click="addBook()">Submit</button>
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
      message: "In books",
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
      this.addBook = book;
      document.querySelector("#book-status").showModal();
    },
    addBook: function () {
      console.log("adding book");
      var params = {
        book_id: this.currentBook,
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

