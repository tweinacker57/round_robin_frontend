<template>
  <div class="profile">
    <h1>{{ message }}</h1>
    <div v-for="bookUser in bookUsers">
      {{ "Title: " + bookUser.book.name }}
      <br>
      {{ "Author: " + bookUser.book.author }}
      <br>
      {{ "Status: " + bookUser.status }}
      <br>
      {{ "Rating: " + bookUser.rating + " stars" }}
      <br>
      <button v-on:click="showBook(bookUser)">Update Status or Rating</button>
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
        <button v-on:click="updateStatus(currentBook)">Update</button>
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
      message: "Profile",
      bookUsers: [],
      currentBook: {},
      status: "",
      rating: "",
    };
  },
  created: function () {
    this.bookUsersIndex();
  },
  methods: {
    bookUsersIndex: function () {
      axios.get("/api/book_users").then((response) => {
        console.log(response.data);
        this.bookUsers = response.data;
      });
    },
    showBook: function (bookUser) {
      console.log("showing book");
      this.currentBook = bookUser;
      document.querySelector("#book-status").showModal();
    },
    updateStatus: function () {
      console.log("updating status");
      var params = {
        status: this.status,
        rating: this.rating,
      };
      console.log(this.currentBook);
      axios
        .patch("/api/book_users/" + this.currentBook.id, params)
        .then((response) => {
          console.log("status update", response);
          this.currentBook = {};
        });
    },
  },
};
</script>

