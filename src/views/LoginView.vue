<template lang="">
  <div class="d-flex align-items-center justify-content-center vh-100 bg-light">
    <div class="card shadow-sm p-4" style="width: 100%; max-width: 400px">
      <h2 class="text-center mb-4">Login</h2>
      <form @submit.prevent>
        <div class="mb-3">
          <label for="email" class="form-label">Email address</label>
          <input
            type="email"
            class="form-control"
            id="email"
            v-model="email"
            placeholder="Enter your email"
            required
          />
        </div>

        <div class="mb-3">
          <label for="password" class="form-label">Password</label>
          <input
            type="password"
            class="form-control"
            id="password"
            v-model="password"
            placeholder="Enter your password"
            required
          />
        </div>

        <button @click="login" class="btn btn-primary w-100">Login</button>
      </form>
    </div>
  </div>
</template>

<script>
import router from "@/router";
import axios from "axios";

export default {
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    login() {
      axios
        .post("http://103.16.117.79:8002/api/auth/login", {
          email: this.email,
          password: this.password,
        })
        .then(function (response) {
          console.log(response);
          localStorage.setItem("email", response.data.data.email);
          localStorage.setItem("name", response.data.data.name);
          localStorage.setItem("role_id", response.data.data.role_id);
          localStorage.setItem("token", response.data.data.token);
          router.push({ name: "home" });
        })
        .catch(function (error) {
          console.log(error);
          console.log("ini error");
        });
    },
  },
  mounted() {
    this.username = localStorage.getItem("name");
    if (this.username) {
      router.push({ name: "home" });
    }
  },
};
</script>

<style scoped lang=""></style>
