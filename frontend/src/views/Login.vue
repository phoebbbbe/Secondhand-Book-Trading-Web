<template>
  <v-container fill-height fluid class="d-flex align-center ">
    <v-row class="d-flex justify-center">
      <v-col cols="4">
        <div class="text-h3 text-left my-3">Login</div>
        <v-form
          ref="form"
          v-model="valid"
          lazy-validation
          class="form"
          @submit.prevent="handleLogin"
        >
          <v-text-field
            v-model="user.email"
            label="Student Number"
            
            outlined
          ></v-text-field>
          <v-text-field
            v-model="user.password"
            label="Password"
            type="password"
            @keyup.enter="handleLogin"
            outlined
          ></v-text-field>
          <div v-show="!valid" class="text-left">
            <span class="red--text text--lighten-1"
              >Student Number or Password error</span
            >
          </div>
          <v-btn
            class="login-btn"
            type="submit"
            :disabled="isLoading"
            color="#f9d600"
            >Submit</v-btn
          >
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      user: {
        email: "",
        password: ""
      },
      valid: true,
      isLoading: false,
      passwordRules: [
        v => !!v || "Password is required",
        v => (v && v.length >= 5) || "password must be more than 5 characters"
      ],
      emailRules: [
        v => !!v || "E-mail is required",
        v => /.+@.+\..+/.test(v) || "E-mail must be valid"
      ]
    };
  },
  methods: {
    async handleLogin() {
      if (this.$refs.form && this.$refs.form.validate()) {
        this.isLoading = true;
        await this.$store
          .dispatch("login/login", [this.user.email, this.user.password])
          .then(() => {
            this.$router.push("/home");
          })
          .catch(err => {
            console.log(err);
            this.isLoading = false;
            this.valid = false;
          });
      }
    }
  }
};
</script>

<style>
.login-btn :hover{
  color: #f0ffff;
  font-size: 16px;
  font-family:Microsoft JhengHei;
  font-weight:bold;

}
.login-btn {
  font-size: 16px;
  font-family:Microsoft JhengHei;
  font-weight:bold;

}
</style>
