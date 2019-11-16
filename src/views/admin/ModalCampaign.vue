<template>
  <div align="right">
    <b-button v-b-modal.modal-campaign variant="primary">Tambah Lokasi Campaign</b-button>
    <b-modal
      id="modal-campaign"
      title="Tambah Lokasi Campaign"
      @cancel="onResetCampaign"
      @ok="onSubmitCampaign"
    >
      <b-form>
        <b-form-group id="input-group-campaign-1" label="Alamat" label-for="input-campaign-1">
         <b-form-select v-model="formCampaign.lokasi" class="mb-3">
            <option
              v-bind:key="lokasi.id"
              v-for="lokasi in list_lokasi"
              v-bind:value="lokasi"
            >{{ lokasi }}</option>
          </b-form-select>
        </b-form-group>
        <b-form-group
          id="input-group-campaign-2"
          label="Target Pembagian"
          label-for="input-campaign-2"
        >
          <b-form-input
            id="input-campaign-2"
            v-model="formCampaign.total_pembagian"
            type="number"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-campaign-3" label="Tanggal" label-for="input-campaign-3">
          <b-form-input id="input-campaign-3" v-model="formCampaign.tanggal" type="date" required></b-form-input>
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
  created(){
    axios.get(host + '/lokasi-terverifikasi')
    .then(res=>{
      var data = res.data.data;
      for (var i in data){
        var lokasi = data[i];
        var nama = lokasi[5] + ", " +lokasi[4] + ", " +lokasi[3] + ", " +lokasi[2] 
        this.list_lokasi.push(nama);
      }
    })
    .catch(e=>{
      alert(e);
    })
  },
  data() {
    return {
      formCampaign: {
        lokasi: "",
        total_pembagian: null,
        tanggal: null
      },
      list_lokasi:[]
    };
  },
  methods: {
    onSubmitCampaign(e) {
      e.preventDefault();
      this.$refs["loading"].show();
      axios
        .post(host + "/create-pembagian", {
          body: this.formCampaign
        })
        .then(response => {
          this.$refs["loading"].hide();
          alert("Berhasil menambahkan campaign");
          window.location.reload();
        })
        .catch(e => {
          this.$refs["loading"].hide();
          alert(e);
          window.location.reload()
        });
    },
    onResetCampaign() {
      this.formCampaign.lokasi = "";
      this.formCampaign.total_pembagian = null;
      this.formCampaign.tanggal = null;
    },
  }
};
</script>

<style>
</style>
