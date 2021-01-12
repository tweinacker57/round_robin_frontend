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
      <br>
      <br>
    </div>
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
        this.bookUsers = response.data;
      });
    },
  },
};
</script>

