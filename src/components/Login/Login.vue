<template>
  <form @click.prevent>
    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <h1>Login</h1>
        <input
          type="email"
          class="form-control"
          placeholder="Enter your email"
          v-model="state.email"
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
          v-model="state.pass"
        />
        <span class="error-feedback" v-if="v$.pass.$error">{{
          v$.pass.$errors[0].$message
        }}</span>
      </div>
    </div>
    <br />

    <div class="row g-3 align-items-center">
      <div class="col-auto d-block mx-auto">
        <button type="submit" @click="userLogin()" class="btn btn-primary">
          login now
        </button>
        &nbsp;&nbsp;&nbsp;
        <button
          type="button"
          @click="redirectTo({ val: 'Signup' })"
          class="btn btn-link"
        >
          sign up
        </button>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto error-feedback">
          {{ userNotFoundError }}
        </div>
      </div>
    </div>
  </form>
</template>
<script>
import axios from "axios";
import { mapActions } from "vuex";
import useValidate from "@vuelidate/core";
import { required, email } from "@vuelidate/validators";
import { reactive, computed } from "vue";
export default {
  name: "LoginForm",
  // compostion API
  setup() {
    const state = reactive({
      email: "",
      pass: "",
    });
    const rules = computed(() => {
      return {
        email: { required, email },
        pass: { required },
      };
    });
    const v$ = useValidate(rules, state);

    return {
      state,
      v$,
    };
  },
  data() {
    return {
      userNotFoundError: "",
    };
  },
  methods: {
    ...mapActions(["redirectTo"]),
    // signUpPage(){
    //     this.$router.push({name: "Signup"})
    // }
    async userLogin() {
      this.v$.$validate();
      if (!this.v$.$error) {
        console.log("form login validate successfully");
        let result = await axios.get(
          `http://localhost:3000/users?email=${this.state.email}&pass=${this.state.pass}`
        );
        if (result.status == 200 && result.data.length > 0) {
          localStorage.setItem("user-info", JSON.stringify(result.data[0]));
          // redirect to home page
          this.redirectTo({ val: "home" });
        } else {
          this.userNotFoundError = "non user found ";
        }
      } else {
        console.log("form login validate fielled");
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
