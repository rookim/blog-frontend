<script>
import axios from "axios";

export default {
  data: function () {
    return {
      // newUserParams: {},
      newUserParams: { name: "", password: "" },
      errors: [],
    };
  },
  methods: {
    submit: function () {
      axios
        .post("/users", this.newUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="signup">
    <form v-on:submit.prevent="submit()">
      <!-- <p>newUserParams: {{ newUserParams }}</p> -->
      <h1>Signup</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Name:</label>
        <input type="text" v-model="newUserParams.name" />
        <br />
        <!-- small tags define smaller text like copyright info and comments -->
        <small>{{ 25 - newUserParams.name.length }} characters remaining</small>
      </div>
      <br />
      <div>
        <label>Email:</label>
        <input type="email" v-model="newUserParams.email" />
      </div>
      <br />
      <div>
        <label>Password:</label>
        <input type="password" v-model="newUserParams.password" />
        <br />
        <small class="danger" v-if="newUserParams.password.length > 20">Must be less than 20 characters!</small>
      </div>
      <br />
      <div>
        <label>Password confirmation:</label>
        <input type="password" v-model="newUserParams.password_confirmation" />
      </div>
      <small
        class="danger"
        v-if="newUserParams.password !== newUserParams.password_confirmation && newUserParams.password.length > 0"
      >
        Passwords do not match!
      </small>
      <br />
      <br />
      <input type="submit" value="Submit" />
    </form>
  </div>
</template>

<!-- what does scoped mean -->
<style scoped>
.danger {
  color: rgb(171, 45, 45);
}
</style>
