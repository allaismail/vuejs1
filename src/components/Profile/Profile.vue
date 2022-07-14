<template>
  <div class="container">
    <Navbar />
    <form @click.prevent>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <h1>My Profile</h1>
          <input
            type="text"
            disabled
            class="form-control"
            placeholder="Enter your name"
            v-model="name"
          />
        </div>
      </div>
      <br />
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <input
            type="email"
            disabled
            class="form-control"
            placeholder="Enter your email"
            v-model="email"
          />
        </div>
      </div>

      <br />
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <input
            type="text"
            disabled
            class="form-control"
            placeholder="Enter your pass"
            v-model="pass"
          />
        </div>
      </div>

      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <button
            type="submit"
            @click="UpdateProfile()"
            class="btn btn-primary"
          >
            updateprofile
          </button>
        </div>
      </div>
    </form>
  </div>
</template>
<script>
import Navbar from "@/components/Header/Navbar.vue";
import { mapActions } from "vuex";
export default {
  name: "ProfileForm",
  data() {
    return {
      name: "",
      email: "",
      pass: "",
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.name = JSON.parse(user).name;
      this.email = JSON.parse(user).email;
      this.pass = JSON.parse(user).pass;
    } else {
      this.redirectTo({ val: "Login" });
    }
  },
  components: {
    Navbar,
  },
  methods: {
    ...mapActions(["redirectTo"]),
    UpdateProfile() {
      this.redirectTo({ val: "UpdateProfile" });
    },
  },
};
</script>
<style></style>
