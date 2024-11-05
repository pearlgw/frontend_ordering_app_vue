<template lang="">
  <Navbar :name="username" :role="roleId" />
  <div>
    {{ username }}
  </div>
</template>
<script>
import Navbar from "@/components/Navbar.vue";
import router from "@/router";

export default {
  components: {
    Navbar,
  },

  data() {
    return {
      username: "",
      roleId: "",
    };
  },

  mounted() {
    this.username = localStorage.getItem("name");
    this.roleId = localStorage.getItem("role_id");
    if (!this.username) {
      router.push({ name: "login" });
    }
  },
};
</script>
<style lang=""></style>
