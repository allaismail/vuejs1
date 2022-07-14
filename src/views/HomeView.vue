<template>
  <Navbar />
  <p class="text-end" id="app">
    welcom {{ userName.toString() }}
    <router-link :to="{ name: 'Profile' }">
      <button class="btn btn-info" type="button">Profile</button>
    </router-link>
  </p>
  <div class="home">Home</div>
</template>
<script>
import { mapActions } from "vuex";
import Navbar from "@/components/Header/Navbar.vue";
export default {
  name: "HomeView",
  data() {
    return {
      userName: "",
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (!user) {
      this.redirectTo({ val: "Login" });
    } else {
      this.userName = JSON.parse(user).name;
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
  },
  components: { Navbar },
};
</script>
