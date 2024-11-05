<template lang="">
  <header v-if="$route.name != 'login'">
    <nav class="navbar navbar-expand-lg bg-dark navbar-dark py-3">
      <div class="container-fluid">
        <RouterLink to="/" class="navbar-brand">RESTO NATAGW</RouterLink>

        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <RouterLink to="/" class="nav-link active">Beranda</RouterLink>
            </li>
            <li class="nav-item" v-if="role == 4 || role == 1">
              <RouterLink to="/order" class="nav-link active">Order</RouterLink>
            </li>
            <li class="nav-item">
              <button @click="logout" class="nav-link btn btn-link text-white">
                Logout
              </button>
            </li>
          </ul>

          <ul class="navbar-nav ms-auto">
            <li class="nav-item">
              <span class="navbar-text text-white-50">Hai, {{ name }}</span>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </header>
</template>
<script>
import router from '@/router';
import axios from 'axios';

export default {
  props: ["name", "role"],
  methods: {
    logout() {
      const token = localStorage.getItem("token");
      
      if (token) {
        axios
          .post(
            "http://103.16.117.79:8002/api/auth/logout",
            {},
            {
              headers: { Authorization: `Bearer ${token}` },
            }
          )
          .then((response) => {
            console.log("Berhasil logout:", response);
            localStorage.removeItem("email");
            localStorage.removeItem("name");
            localStorage.removeItem("role_id");
            localStorage.removeItem("token");
            router.push({ name: "login" });
          })
          .catch((error) => {
            console.error("Gagal logout:", error);
            console.log("Terjadi kesalahan saat logout");
          });
      } else {
        console.log("Token tidak ditemukan, tidak bisa logout.");
        router.push({ name: "login" });
      }
    },
  },
};
</script>
