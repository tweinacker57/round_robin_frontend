<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="bookUser in bookUsers">
      {{ bookUser.book.name }}

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
      message: "Round Robin- The Book App",
      bookUsers: [],
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

