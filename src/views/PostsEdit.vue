<script>
import axios from "axios";

export default {
  data: function () {
    return {
      editPostParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log("Post ID: ", this.$route.params.id);
      console.log("Post Object:", response.data);
      this.editPostParams = response.data;
    });
  },
  methods: {
    editPost: function () {
      axios
        .patch(`/posts/${this.$route.params.id}`, this.editPostParams)
        .then((response) => {
          console.log("Edited Post:", response.data);
          this.$router.push(`/posts/${response.data.id}`);
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="posts-new">
    <form v-on:submit.prevent="editPost()">
      <h1>Edit Post</h1>
      <!-- <p>{{ editPostParams }}</p> -->
      <img v-bind:src="editPostParams.image" alt="" />
      <div>
        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
      </div>
      <div>
        <label>Title:</label>
        <input type="text" v-model="editPostParams.title" />
      </div>
      <div>
        <label>Body:</label>
        <input type="text" v-model="editPostParams.body" />
      </div>
      <div>
        <label>Image:</label>
        <input type="text" v-model="editPostParams.image" />
      </div>
      <br />
      <input type="submit" value="Update" />
    </form>
  </div>
</template>
