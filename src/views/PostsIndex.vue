<script>
// this file is a component
import axios from "axios";

export default {
  data: function () {
    return {
      posts: [],
      titleFilter: "",
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      console.log("Posts Index:", response.data);
      this.posts = response.data;
    });
  },
  // review this
  computed: {
    filteredPosts() {
      return this.posts.filter((post) => {
        return post.title.toLowerCase().includes(this.titleFilter.toLowerCase());
      });
    },
  },
};
</script>

<template>
  <div class="posts-index"></div>
  <h1>Blog Posts</h1>
  <!-- review this: search function -->
  <p>
    Search:
    <input type="text" v-model="titleFilter" list="postTitles" />
  </p>
  <datalist id="postTitles">
    <option v-for="post in posts" v-bind:key="post.id">{{ post.title }}</option>
  </datalist>
  <br />
  <br />
  <!-- changed posts to filteredPosts to accomodate search function -->
  <transition-group
    appear
    enter-active-class="animate__animated animate__fadeIn"
    leave-active-class="animate__animated animate__fadeOut"
  >
    <div v-for="post in filteredPosts" v-bind:key="post.id">
      <router-link v-bind:to="`/posts/${post.id}`">
        <img :src="post.image" alt="post.title" />
      </router-link>
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
      <br />
      <br />
    </div>
  </transition-group>
</template>
