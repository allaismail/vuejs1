<template>
  <Navbar />
  <div class="container">
    <form @click.prevent>
      <div class="row g-3 align-items-center">
        <h1 class="row  g-3">update your information !!!</h1>
        <div class="col-auto d-block mx-auto">
          <input type="text" class="form-control" placeholder="Enter your name" v-model="name" />
          <span class="error-feedback" v-if="v$.name.$error">{{
              v$.name.$errors[0].$message
          }}</span>
        </div>
      </div>
      <br />
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <input type="email" class="form-control" placeholder="Enter your email" v-model="email" />
          <span class="error-feedback" v-if="v$.email.$error">{{
              v$.email.$errors[0].$message
          }}</span>
        </div>
      </div>

      <br />
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <input type="text" class="form-control" placeholder="Enter your pass" v-model="pass" />
          <span class="error-feedback" v-if="v$.pass.$error">{{
              v$.pass.$errors[0].$message
          }}</span>
        </div>
      </div>

      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <button type="submit" @click="UpdateProfileNow()" class="btn btn-primary">
            update Now !
          </button>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto error-feedback">
          {{ updateError }}
        </div>
      </div>
    </form>

  </div>
</template>
<script>
import Navbar from "@/components/Header/Navbar.vue";
import useValidate from "@vuelidate/core";
import axios from "axios";
import { mapActions } from "vuex";
import { required, email, minLength } from "@vuelidate/validators";
export default {
  name: "UpdateProfile",
  data() {
    return {
      v$: useValidate(),
      name: "",
      email: "",
      pass: "",
      userId: "",
      updateError: "",
    };
  },
  validations() {
    return {
      name: { required, minLength: minLength(4) },
      email: { required, email },
      pass: { required, minLength: minLength(4) },
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.name = JSON.parse(user).name;
      this.email = JSON.parse(user).email;
      this.pass = JSON.parse(user).pass;
      this.userId = JSON.parse(user).id;
    } else {
      this.redirectTo({ val: "Login" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    async UpdateProfileNow() {
      this.v$.$validate();
      if (!this.v$.$error) {
        let result = await axios.put(`http://localhost:3000/users/${this.userId}`,
          {
            name: this.name,
            email: this.email,
            pass: this.pass,

          });

        if (result.status == 200) {
          console.log("Profile is updated successflly");
          localStorage.setItem("user-info", JSON.stringify(result.data));
          this.redirectTo({ val: "home" });
        } else {
          this.updateError = "something went wrang , try again !!!";
        }
      } else {
        this.updateError = "something went wrang , refresh the page !!!";
      }
    },
  },

  components: {
    Navbar,
  },
};
</script>
<style scoped>
.error-feedback {
  color: red;
  font-size: 0.85em;
}
</style>

