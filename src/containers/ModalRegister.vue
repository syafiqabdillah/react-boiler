<template>
  <div>
    <b-modal
      id="register"
      title="Register"
      @shown="focusMyElement"
      @cancel="onResetRegister"
      @ok="onSubmitRegister"
    >
      <b-form @keypress.enter="onSubmitRegister">
        <b-form-group id="input-group-register-1" label="Nama" label-for="input-register-1">
          <b-form-input
            id="input-register-1"
            v-model="formRegister.name"
            type="text"
            required
            ref="focusThis"
          ></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-register-3" label="Username" label-for="input-register-3">
          <b-form-input id="input-register-3" v-model="formRegister.username" type="text" required></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-register-4" label="Password" label-for="input-register-4">
          <b-form-input
            id="input-register-4"
            v-model="formRegister.password"
            type="password"
            required
          ></b-form-input>
        </b-form-group>
      </b-form>
    </b-modal>
    <b-modal ref="loading" hide-header hide-footer>
      <div align="center">
        <b-spinner label="Spinning"></b-spinner>
        <br />Loading...
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
      formRegister: {
        name: "",
        username: "",
        password: ""
      }
    };
  },
  methods: {
    onSubmitRegister(e) {
      e.preventDefault();
      if (
        this.formRegister.username == "" ||
        this.formRegister.password == "" ||
        this.formRegister.name == ""
      ) {
        alert("Mohon lengkapi form register");
      } else {
        this.$refs["loading"].show();
        axios
          .post(host + "/register", {
            body: this.formRegister
          })
          .then(response => {
            this.$refs["loading"].hide();
            var data = response.data;
            sessionStorage.setItem("id", data.id);
            sessionStorage.setItem("nama", data.name);
            window.location.reload();
          })
          .catch(e => {
            alert(
              "Username ini telah terpakai oleh orang lain, coba username lain"
            );
            this.$refs["loading"].hide();
          });
      }
    },
    onResetRegister() {
      this.formCampaign.name = "";
      this.formCampaign.username = "";
      this.formCampaign.password = "";
    },
    focusMyElement() {
      this.$refs.focusThis.focus();
    }
  }
};
</script>
<style>
</style>