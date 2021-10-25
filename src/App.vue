<template>
  <div class="main">
    <input
      class="main__input"
      type="text"
      placeholder="Filter by author..."
      @input="handleInput"
    />
    <ul class="posts-list list">
      <li
        class="posts-list__item"
        v-for="post in posts"
        :key="post.id"
        :id="post.userId"
      >
        <h3 class="posts-list__headline">{{ post.title }}</h3>
        <p class="posts-list__description">{{ post.body }}</p>
        <ul class="users-list list ul">
          <li
            class="users-list__item none"
            v-for="user in users"
            :key="user.id"
            :class="{ block: user.id === post.userId }"
          >
            <p class="users-list__author" :id="post.userId">{{ user.name }}</p>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      posts: null,
      users: null,
      message: "",
    };
  },

  methods: {
    handleInput: function (event) {
      let posts = document.querySelectorAll(".posts-list__item");
      let users = document.querySelectorAll(".users-list__item");
      let patternRegExp = new RegExp(event.target.value);

      posts.forEach(function (post) {
        post.classList.add("none");
        users.forEach(function (user) {
          if (user.classList.contains("block")) {
            let author = user.querySelector(".users-list__author");
            if (patternRegExp.test(author.innerText) && author.id === post.id) {
              post.classList.remove("none");
            }
          }
        });
      });
    },
  },

  mounted() {
    axios
      .get("http://jsonplaceholder.typicode.com/posts")
      .then((response) => (this.posts = response.data));

    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((response) => (this.users = response.data));
  },
};
</script>
