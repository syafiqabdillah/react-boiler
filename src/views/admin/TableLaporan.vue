<template>
  <b-card>
    <b-table
      id="my-table-laporan"
      show-empty
      :items="items_laporan"
      :fields="fields_laporan"
      :current-page="currentPage"
      :per-page="perPage"
      responsive
    >
      <template slot="actions" slot-scope="row">
        <b-button
          size="sm"
          v-b-modal.edit-campaign
          variant="primary"
          @click="selectCampaign(row.item)"
        >EDIT</b-button>
      </template>
    </b-table>
    <b-row>
      <b-col sm="6" lg="6">
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          aria-controls="my-table-laporan"
          :per-page="perPage"
        ></b-pagination>
      </b-col>
      <b-col sm="6" lg="6">
        <ModalCampaign />
      </b-col>
    </b-row>

    <b-modal id="edit-campaign" title="Edit Campaign" @ok="submitEditCampaign" >
      <b-form>
        <b-form-group id="input-group-campaign-1" label="Lokasi" label-for="input-campaign-1">
          <b-form-input
            id="input-campaign-1"
            v-model="formCampaign.lokasi"
            type="text"
            disabled
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-campaign-2"
          label="Target Pembagian"
          label-for="input-campaign-2"
        >
          <b-form-input disabled id="input-campaign-2" v-model="formCampaign.target" type="number" required></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-campaign-3"
          label="Realisasi Pembagian"
          label-for="input-campaign-3"
        >
          <b-form-input
            id="input-campaign-3"
            v-model="formCampaign.realisasi"
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
  </b-card>
</template>

<script>
import axios from "axios";
const host = process.env.VUE_APP_HOST;
import ModalCampaign from "./ModalCampaign";
export default {
  created() {
    axios.get(host + "/all-pembagian").then(res => {
      for (var i in res.data.data){
        var date = new Date(res.data.data[i][2]);
        res.data.data[i][2] = date.toDateString();
      }
      this.items_laporan = res.data.data.reverse();
    });
  },
  components: {
    ModalCampaign
  },
  data() {
    return {
      items_laporan: [],
      fields_laporan: [
        { key: "3", label: "Lokasi" },
        { key: "4", label: "Target Pembagian" },
        { key: "1", label: "Realisasi Pembagian" },
        { key: "2", label: "Tanggal" },
        "actions"
      ],
      currentPage: 1,
      perPage: 3,
      formCampaign: {
        id: null,
        lokasi: "",
        target: null,
        realisasi: null,
        tanggal: null
      }
    };
  },
  computed: {
    rows() {
      return this.items_laporan.length;
    }
  },
  methods: {
    focusMyElement() {
      this.$refs.focusThis.focus();
    },
    selectCampaign(item) {
      this.formCampaign.id = item[0];
      this.formCampaign.lokasi = item[3];
      this.formCampaign.target = item[4];
      this.formCampaign.realisasi = item[1];
      this.formCampaign.tanggal = item[2];
    },
    submitEditCampaign(){
      this.$refs["loading"].show();
      axios.post(host+'/edit-campaign',{
        body: this.formCampaign
      })
      .then(res=>{
        this.$refs["loading"].hide();
        alert("Berhasil");
        window.location.reload();
      })
      .catch(e=>{
        this.$refs["loading"].hide();
        alert(e);
        window.location.reload();
      })
    }
  }
};
</script>

<style>
</style>
