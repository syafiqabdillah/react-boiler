<template>
  <div>
    <b-modal
      id="login"
	  title="Login"
      @shown="focusMyElement"
      @cancel="onResetLogin"
      @ok="onSubmitLogin"
    >
      <b-form @keypress.enter="onSubmitLogin">
        <b-form-group id="input-group-login-1" label="Username" label-for="input-login-1">
          <b-form-input
            id="input-login-1"
            v-model="formLogin.username"
            type="text"
            required
            ref="focusThis"
          ></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-login-2" label="Password" label-for="input-login-2">
          <b-form-input id="input-login-2" v-model="formLogin.password" type="password" required></b-form-input>
        </b-form-group>
      </b-form>
      <div align="right">belum punya akun ? <a href="#" v-b-modal.register>Register</a> </div>
    </b-modal>
    <b-modal ref="loading" hide-header hide-footer>
      <div align="center">
        <b-spinner label="Spinning"></b-spinner>
        <br>
        Loading...
      </div>
    </b-modal>
  </div>
</template>

<script>
import axios from "axios";
const host = process.env.VUE_APP_HOST;
export default {
  data() {
    return {
      formLogin: {
        username: "",
        password: ""
      }
    };
  },
  methods: {
    onSubmitLogin(e) {
      e.preventDefault();
      if (this.formLogin.username == "" || this.formLogin.password == "") {
        alert("Mohon lengkapi form login");
      } else {
        this.$refs["loading"].show();
        axios
          .post(host + "/login", {
            body: this.formLogin
          })
          .then(response => {
            this.$refs["loading"].hide();
            if (response.data.hasOwnProperty("msg")) {
              if (response.data.msg == "username not found") {
                alert("Username tidak ditemukan");
              } else {
                alert("Password salah");
              }
            } else {
              sessionStorage.setItem("nama", response.data.name);
              sessionStorage.setItem("id", response.data.id);
              sessionStorage.setItem("role", response.data.role);
              window.location.reload();
            }
          })
          .catch(e => {
            this.$refs["loading"].hide();
            alert(e);
          });
      }
    },
    onResetLogin() {
      this.formLogin.username = "";
      this.formLogin.password = "";
    },
    focusMyElement() {
      this.$refs.focusThis.focus();
    }
  },
  props: {
    propMessage: {
      type: String,
      default: "Login"
    }
  }
};
</script>

<style>
</style>
