<template>
  <div class="animated fadeIn" style="margin:15px">
    <div align="center">
      <h2>MENJADI RELAWAN</h2>
      <p>Kamu dapat membantu kami menebar kebaikan bersama</p>
    </div>
    <b-row align-h="center">
      <b-col lg="5" sm="12">
        <b-card>
          <div align="center">
            <h3>Formulir Pendaftaran</h3>
          </div>
          <b-form @submit="onSubmitPendaftaran">
            <b-form-group
              id="input-group-bergabung-1"
              label="Nama Lengkap"
              label-for="input-bergabung-1"
            >
              <b-form-input
                id="input-bergabung-1"
                v-model="formPendaftaran.nama"
                type="text"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group id="input-group-bergabung-2" label="No HP" label-for="input-bergabung-2">
              <b-form-input
                id="input-bergabung-2"
                v-model="formPendaftaran.no_hp"
                type="text"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group id="input-group-bergabung-3" label="Email" label-for="input-bergabung-3">
              <b-form-input
                id="input-bergabung-3"
                v-model="formPendaftaran.email"
                type="email"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group id="input-group-bergabung-5" label="Alamat" label-for="input-bergabung-5">
              <b-form-input
                id="input-bergabung-5"
                v-model="formPendaftaran.alamat"
                type="text"
                required
              ></b-form-input>
            </b-form-group>
            <b-form-group
              id="input-group-bergabung-4"
              label="Lokasi Campaign Pilihan"
              label-for="input-bergabung-4"
            >
              <b-form-select v-model="formPendaftaran.lokasi" class="mb-3">
                <option
                  v-bind:key="lokasi.id"
                  v-for="lokasi in list_lokasi"
                  v-bind:value="lokasi"
                >{{ lokasi }}</option>
              </b-form-select>
            </b-form-group>
            <b-button
              style="background-color:#FFA63D;color:white;border:0px;"
              block
              type="submit"
            >Submit</b-button>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
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
      formPendaftaran: {
        nama: "",
        alamat: "",
        no_hp: "",
        email: "",
        lokasi: ""
      },
      list_lokasi: []
    };
  },
  methods: {
    onSubmitPendaftaran(e) {
      e.preventDefault();
      this.$refs["loading"].show();
      axios
        .post(host + "/bergabung", {
          body: this.formPendaftaran
        })
        .then(res => {
          this.$refs["loading"].hide();
          alert(
            "Lamaran bergabung berhasil dibuat. Tunggu admin kami menghubungi Anda"
          );
          window.location.reload();
        })
        .catch(e => {
          this.$refs["loading"].hide();
          alert(e);
          window.location.reload();
        });
    }
  },
  created() {
    if (sessionStorage.getItem("nama") !== null) {
      this.formPendaftaran.nama = sessionStorage.getItem("nama");
    }
    axios
      .get(host + "/lokasi-terverifikasi")
      .then(res => {
        var data = res.data.data;
        for (var i in data) {
          var lokasi = data[i];
          var nama =
            lokasi[5] + ", " + lokasi[4] + ", " + lokasi[3] + ", " + lokasi[2];
          this.list_lokasi.push(nama);
        }
      })
      .catch(e => {
        alert(e);
      });
  }
};
</script>

<style>
</style>
