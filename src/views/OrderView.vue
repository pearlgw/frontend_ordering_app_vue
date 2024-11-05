<template lang="">
  <Navbar :name="username" :role="roleId" />
  <div class="container mt-5">
    <div class="row">
      <!-- Kolom Kiri -->
      <div class="col-12 col-sm-8">
        <!-- Input Search -->
        <div class="mb-3">
          <input
            type="text"
            v-model="keyword"
            class="form-control"
            placeholder="Search Here"
            :onchange="searchItem()"
          />
        </div>

        <!-- Card Items -->
        <div class="row">
          <div
            class="col-12 col-sm-6 col-md-4 mb-3"
            v-for="item in filteredImage"
            :key="item.id"
          >
            <div class="card">
              <img
                :src="url + item.image"
                height="150px"
                class="card-img-top object-fit-cover"
                alt="..."
              />
              <div class="card-body text-center">
                <h5 class="card-title">{{ item.name }}</h5>
                <p class="card-text">{{ `Rp ${item.price}` }}</p>
                <button class="btn btn-success">Order</button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Kolom Kanan -->
      <div class="col-12 col-sm-4 bordered mb-3">
        <h5>Konten Kolom Kanan</h5>
        <p>kanan</p>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import router from "@/router";
import axios from "axios";

export default {
  components: {
    Navbar,
  },

  data() {
    return {
      username: "",
      roleId: "",
      items: [],
      filteredImage: [],
      keyword: "",
      url: "http://103.16.117.79:8002/storage/",
    };
  },

  mounted() {
    this.username = localStorage.getItem("name");
    this.roleId = localStorage.getItem("role_id");
    
    if (!this.username) {
      router.push({ name: "login" });
    }

    if (this.roleId != 1 && this.roleId != 4) {
      router.push({ name: "home" });
    }

    this.getItems();
  },
  methods: {
    getItems() {
      axios
        .get("http://103.16.117.79:8002/api/items", {
          headers: { Authorization: `Bearer ${localStorage.getItem("token")}` },
        })
        .then((response) => {
          //   console.log(response.data.data);
          this.items = response.data.data;
        })
        .catch(function (error) {
          console.log(error);
          console.log("ini error");
        });
    },
    searchItem() {
      this.filteredImage = this.items.filter((item) =>
        item.name.toLowerCase().includes(this.keyword.toLowerCase())
      );
    },
  },
};
</script>

<style scoped>
.bordered {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 5px;
}
</style>
