<script>
// this file is a component
import axios from "axios";

export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log("Post ID: ", this.$route.params.id);
      console.log("Post Object:", response.data);
      this.post = response.data;
    });
  },
  methods: {
    destroyPost: function () {
      if (confirm("Are you sure you want to delete this?")) {
        axios.delete(`/posts/${this.$route.params.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/posts");
        });
      }
    },
  },
};
</script>

<template>
  <div class="posts-show"></div>
  <br />
  <br />
  <img :src="post.image" alt="post.title" />
  <h2>{{ post.title }}</h2>
  <p>{{ post.body }}</p>
  <button><router-link v-bind:to="`/posts/${post.id}/edit`">Edit</router-link></button>
  <button v-on:click="destroyPost()">Delete</button>
  <br />
  <br />
</template>
