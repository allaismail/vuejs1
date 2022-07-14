<template>
  <form @click.prevent>
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <h1>Sign up</h1>
        <input
          type="text"
          class="form-control"
          placeholder="Enter your name"
          v-model="name"
        />
        <span class="error-feedback" v-if="v$.name.$error">{{
          v$.name.$errors[0].$message
        }}</span>
      </div>
    </div>
    <br />
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <input
          type="email"
          class="form-control"
          placeholder="Enter your email"
          v-model="email"
        />
        <span class="error-feedback" v-if="v$.email.$error">{{
          v$.email.$errors[0].$message
        }}</span>
      </div>
    </div>
    <br />
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <input
          type="password"
          class="form-control"
          placeholder="Enter your pass"
          v-model="pass"
        />
        <span class="error-feedback" v-if="v$.pass.$error">{{
          v$.pass.$errors[0].$message
        }}</span>
      </div>
    </div>
    <br />

    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <button type="submit" @click="signUpNow()" class="btn btn-primary">
          sign up now
        </button>
        &nbsp;&nbsp;&nbsp;
        <button
          type="button"
          @click="redirectTo({ val: 'Login' })"
          class="btn btn-link"
        >
          login
        </button>
      </div>
    </div>
  </form>
</template>
<script>
import axios from "axios";
import { mapActions } from "vuex";
import useValidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";

export default {
  name: "SignUpForm",
  data() {
    return {
      v$: useValidate(),
      name: "",
      email: "",
      pass: "",
    };
  },
  validations() {
    return {
      name: { required },
      pass: { required },
      email: { required, email },
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.redirectTo({ val: "home" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    // loginPage(){
    //     this.$router.push({name: "Login"})
    // }
    async signUpNow() {
      this.v$.$validate();
      if (!this.v$.$error) {
        console.log("form validate successfully");
        let result = await axios.post("http://localhost:3000/users", {
          name: this.name,
          email: this.email,
          pass: this.pass,
        });
        if (result.status == 201) {
          console.log("data added successfully");
          // save data in local storage
          localStorage.setItem("user-info", JSON.stringify(result.data));
          console.log(result);
          console.log(JSON.stringify(result.data));
          // redirect to home page
          this.redirectTo({ val: "home" });
        } else {
          console.log("error to added the data ");
        }
      } else {
        console.log("form validate fielled");
      }
    },
  },
};
</script>
<style scoped>
.error-feedback {
  color: red;
  font-size: 0.85em;
}
</style>
