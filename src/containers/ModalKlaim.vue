<template>
  <div>
    <b-modal
      id="klaim"
      title="Klaim Donasi"
      @cancel="onResetKlaim"
      @ok="onSubmitKlaim"
    >
      <b-form @submit="onSubmitKlaim">
        <b-form-group
          id="input-group-klaim-1"
          label="Scan/foto bukti transfer"
          label-for="input-klaim-1"
        >
          <b-form-file
            v-model="formKlaim.file"
            id="input-group-klaim-1"
            placeholder="Choose a file..."
            drop-placeholder="Drop file here..."
            accept="image/jpeg, image/png"
          ></b-form-file>
        </b-form-group>
        <b-form-group id="input-group-klaim-2" label="Pesan" label-for="input-klaim-2">
          <b-form-input id="input-klaim-2" v-model="formKlaim.pesan"></b-form-input>
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
      formKlaim: {
        id_donatur: sessionStorage.getItem("id"),
        file: null,
        pesan: ""
      }
    };
  },
  methods: {
    onSubmitKlaim(e) {
      e.preventDefault();
      if (this.formKlaim.file == null) {
        alert("Mohon upload bukti transfer sebelum melanjutkan");
      } else {
        this.$refs["loading"].show();
        let url = host + "/klaim";
        let form = new FormData();
        let file = this.formKlaim.file;
        let pesan = this.formKlaim.pesan;
        let id_donatur = this.formKlaim.id_donatur;
        form.append("nama", file.name);
        form.append("file", file);
        form.append("pesan", pesan);
        form.append("id_donatur", id_donatur);
        let config = {
          header: {
            "Content-Type": "multipart/form-data"
          }
        };
        axios
          .post(url, form, config)
          .then(response => {
            this.$refs["loading"].hide();
            if(!alert("Klaim berhasil diajukan dan akan segera kami verifikasi")){
              window.location.reload();
            }
          })
          .catch(e => {
            this.$refs["loading"].hide();
            alert(e);
          });
      }
    },
    onResetKlaim() {
      this.formKlaim.pesan = "";
      this.formKlaim.file = null;
    }
  }
};
</script>

<style>
</style>
