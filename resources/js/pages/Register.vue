<template>
  <div class="container">
    <h2 class="register-title">Register</h2>
    <div class="card-body">
      <div class="col-md-6 offset-md-3">
        <form v-on:submit.prevent="onSubmit">
          <div class="alert alert-danger" v-if="errors.length">
            <ul class="mb-0">
              <li v-for="(error, index) in errors" :key="index">{{ error }}</li>
            </ul>
          </div>
          <div class="form-group">
            <label>Name</label>
            <input type="text" class="form-control" placeholder="Name..." v-model="name" />
          </div>
          <div class="form-group">
            <label>Username</label>
            <input type="text" class="form-control" placeholder="Username..." v-model="username" />
          </div>
          <div class="form-group">
            <label>Password</label>
            <input
              type="password"
              class="form-control"
              placeholder="Password..."
              v-model="password"
            />
          </div>
          <div class="form-group">
            <label>Confirm Password</label>
            <input
              type="password"
              class="form-control"
              placeholder="Confirm Password..."
              v-model="passwordAgain"
            />
          </div>

          <button class="btn">Register</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "register",
  props: ["app"],
  data() {
    return {
      name: "",
      username: "",
      password: "",
      passwordAgain: "",
      errors: []
    };
  },

  methods: {
    onSubmit() {
      this.errors = [];

      if (!this.name) {
        this.errors.push("Name is required.");
      }

      if (!this.username) {
        this.errors.push("Username is required.");
      }

      if (!this.password) {
        this.errors.push("Password is required.");
      }

      if (!this.passwordAgain) {
        this.errors.push("Password confirmation is required.");
      }

      if (!this.password !== !this.passwordAgain) {
        this.errors.push("Passwords do not matched !");
      }

      if (!this.errors.length) {
        const data = {
          name: this.name,
          username: this.username,
          password: this.password
        };

        this.app.req
          .post("auth/register", data)
          .then(response => {
            this.app.user = response.data;
            this.$router.push("/");
          })
          .catch(error => {
            this.errors.push(error.response.data.error);
          });
      }
    }
  }
};
</script>

<style scoped>
label {
  color: white;
  font-family: "Quicksand", Tahoma, Geneva, Verdana, sans-serif;
}
button {
  background-color: transparent;
  border-color: orange;
  color: white;
}
.btn:hover {
  color: orange !important;
}
.register-title {
  color: orange !important;
  font-size: 3vw;
  margin-top: 20px;
  font-family: "Bebas Neue", cursive;
}
</style>