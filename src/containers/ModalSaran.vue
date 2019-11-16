<template>
  <div>
    <b-modal id="saran" title="Saran Lokasi" @cancel="onResetSaran" @ok="onSubmitSaran">
      <b-form @keypress.enter="onSubmitSaran">
        <b-form-group label="Provinsi">
          <b-form-select v-model="formSaran.provinsi" class="mb-3" @change="getKabkota">
            <option
              v-bind:key="prov.id"
              v-for="prov in list_provinsi"
              v-bind:value="prov"
            >{{ prov.name }}</option>
          </b-form-select>
        </b-form-group>

        <b-form-group label="Kabupaten / Kota">
          <b-form-select v-model="formSaran.kabkota" class="mb-3" @change="getKecamatan">
            <option
              v-bind:key="kota.id"
              v-for="kota in list_kabkota"
              v-bind:value="kota"
            >{{ kota.name }}</option>
          </b-form-select>
        </b-form-group>

        <b-form-group label="Kecamatan">
          <b-form-select v-model="formSaran.kecamatan" class="mb-3" @change="getKelurahan">
            <option
              v-bind:key="kec.id"
              v-for="kec in list_kecamatan"
              v-bind:value="kec"
            >{{ kec.name }}</option>
          </b-form-select>
        </b-form-group>
        <b-form-group label="Kelurahan">
          <b-form-select v-model="formSaran.kelurahan" class="mb-3">
            <option
              v-bind:key="kel.id"
              v-for="kel in list_kelurahan"
              v-bind:value="kel"
            >{{ kel.name }}</option>
          </b-form-select>
        </b-form-group>
      </b-form>
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
//UKariITw8MXbTKzGFR9tZWj7CaOcqrIS0eZjBCBMDxloB465Cb
export default {
  created() {
    axios.get("https://x.rajaapi.com/poe")
    .then(response =>{
      sessionStorage.setItem("kodeunik", response.data.token);
    }).catch(e => {
      alert(e);
    })

    axios
      .get(
        "https://x.rajaapi.com/MeP7c5ne" + this.kodeunik + "/m/wilayah/provinsi"
      )
      .then(response => {
        this.list_provinsi = response.data.data;
      })
      .catch(e => {
        alert(e);
      });
  },
  data() {
    return {
      formSaran: {
        provinsi: null,
        kabkota: null,
        kecamatan: null,
        kelurahan: null
      },
      list_provinsi: [],
      list_kabkota: [],
      list_kecamatan: [],
      list_kelurahan: [],
      selected: null,
    };
  },
  computed: {
    fieldKabkot() {
      return this.formSaran.provinsi !== "";
    },
    kodeunik(){
      return sessionStorage.getItem("kodeunik");
    }
  },
  methods: {
    onSubmitSaran(e) {
      e.preventDefault();

      var form = this.formSaran;
      if (
        form.provinsi == null ||
        form.kabkota == null ||
        form.kecamatan == null ||
        form.kelurahan == null
      ) {
        alert("Mohon lengkapi form saran");
      }
      else {
        this.$refs["loading"].show();
        axios
          .post(host + "/saran-lokasi", {
            body: {
              "id_donor":sessionStorage.getItem("id"),
              "provinsi":form.provinsi.name,
              "kabkota":form.kabkota.name,
              "kecamatan":form.kecamatan.name,
              "kelurahan":form.kelurahan.name,
              "kodepos":"1"
            }
          })
          .then(response => {
            this.$refs["loading"].hide();
            alert("Saran berhasil ditambahkan");
            window.location.reload();
          })
          .catch(e => {
            this.$refs["loading"].hide();
            alert(e);
          });
      }
    },
    onResetSaran() {
      this.formSaran.provinsi = null;
      this.formSaran.kabkota = null;
      this.formSaran.kecamatan = null;
      this.formSaran.kelurahan = null;
    },
    getKabkota() {
      axios
        .get(
          "https://x.rajaapi.com/MeP7c5ne" + this.kodeunik + "/m/wilayah/kabupaten?idpropinsi=" +
            this.formSaran.provinsi.id
        )
        .then(response => {
          this.list_kabkota = response.data.data;
        });
    },
    getKecamatan() {
      axios
        .get(
          "https://x.rajaapi.com/MeP7c5ne" + this.kodeunik + "/m/wilayah/kecamatan?idkabupaten=" +
            this.formSaran.kabkota.id
        )
        .then(response => {
          this.list_kecamatan = response.data.data;
        });
    },
    getKelurahan() {
      axios
        .get(
          "https://x.rajaapi.com/MeP7c5ne" + this.kodeunik + "/m/wilayah/kelurahan?idkecamatan=" +
            this.formSaran.kecamatan.id
        )
        .then(response => {
          this.list_kelurahan = response.data.data;
        });
    }
  }
};
</script>

<style>
</style>
