<template>
  <div class="profile">
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
      message: "Profile",
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

