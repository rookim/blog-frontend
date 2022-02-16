<script>
import axios from "axios";

export default {
  data: function () {
    return {
      newPostParams: {},
      errors: [],
      sadStatus: "",
    };
  },
  methods: {
    createPost: function () {
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log("New Post:", response.data);
          this.$router.push("/posts");
        })
        .catch((error) => {
          this.sadStatus = error.response.status;
          this.errors = error.response.data.errors;
          console.log(this.sadStatus, this.errors);
        });
    },
  },
};
</script>

<template>
  <div class="posts-new">
    <form v-on:submit.prevent="createPost()">
      <h1>Create a Post</h1>
      <!-- use error cat pics api -->
      <img v-if="sadStatus" :src="`https://http.cat/${sadStatus}`" alt="" />
      <div>
        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
      </div>
      <div>
        <label>Title:</label>
        <input type="text" v-model="newPostParams.title" />
      </div>
      <div>
        <label>Body:</label>
        <input type="text" v-model="newPostParams.body" />
      </div>
      <div>
        <label>Image:</label>
        <input type="text" v-model="newPostParams.image" />
      </div>
      <br />
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>
