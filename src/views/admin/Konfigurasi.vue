<template>
  <div>
    <b-card>
      <b-form @submit="onClickUbah">
        <b-form-group
          id="input-group-konfig-1"
          label="Email Admin untuk Verifikasi Klaim"
          label-for="input-konfig-1"
        >
          <b-form-input
            id="input-konfig-1"
            v-model="config.email_admin"
            type="text"
            v-b-tooltip.hover
            title="Bukti transfer yang dikirimkan oleh donatur akan menuju ke email ini"
          ></b-form-input>
        </b-form-group>
        <b-btn type="submit" variant="primary" block>SUBMIT</b-btn>
      </b-form>
    </b-card>
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
  created() {
    axios.get(host + "/konfigurasi").then(res => {
      this.config.email_admin = res.data.data[0][0];
    });
  },
  data() {
    return {
      config: {
        email_admin: ""
      }
    };
  },
  methods: {
    onClickUbah(e) {
      e.preventDefault();
      this.$refs["loading"].show();
      axios
        .post(host + "/ubah-konfigurasi", {
          body: this.config
        })
        .then(res => {
          this.$refs["loading"].hide();
          alert("Perubahan konfigurasi berhasil disimpan");
          window.location.reload();
        })
        .catch(e => {
          this.$refs["loading"].hide();
          alert(e);
        });
    }
  }
};
</script>

<style>
</style>
