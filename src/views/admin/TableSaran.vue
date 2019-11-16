<template>
  <div class="animated fadeIn">
    <b-card>
      <b-table
        :fields="list_field"
        :items="list_saran"
        :current-page="currentPage"
        :per-page="perPage"
        id="my-table-saran"
        responsive
        show-empty
      >
        <template slot="actions" slot-scope="row">
          <b-button
            v-b-modal.terima-saran
            class="mr-1"
            size="sm"
            variant="primary"
            @click="selectSaran(row.item)"
          >TERIMA</b-button>
          <!-- <b-button
            size="sm"
            v-b-modal.tolak-saran
            variant="danger"
            @click="selectSaran(row.item)"
          >TOLAK</b-button> -->
        </template>
      </b-table>
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        aria-controls="my-table-saran"
        :per-page="perPage"
      ></b-pagination>
    </b-card>
    <b-modal id="terima-saran" title="Konfirmasi" @ok="onSubmitTerimaSaran" >
      <div align="center">Yakin akan menerima saran lokasi {{ formSaran.lokasi }} ?</div>
    </b-modal>
    <b-modal id="tolak-saran" title="Konfirmasi" @ok="onSubmitTolakSaran">
      <div align="center">Yakin akan menolak saran lokasi {{ formSaran.lokasi }} ?</div>
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
      options: ["Diproses", "Terverifikasi"],
      list_saran: [],
      list_field: [
        { key: "nama", label: "Alamat" },
        { key: "status", label: "Status" },
        "actions"
      ],
      selected: null,
      currentPage: 1,
      perPage: 6,
      formSaran: {
        id:null,
        lokasi: "",
        status: ""
      }
    };
  },
  computed: {
    rows() {
      return this.list_saran.length;
    }
  },
  created() {
    axios.get(host + "/all-saran-lokasi").then(res => {
      var result = [];
      for (var i in res.data.data) {
        var item = res.data.data[i];
        var nama = "KELURAHAN " + item[5];
        let status = "";
        if (item[1] == 1) {
          status = "Diproses";
        } else {
          status = "Terverifikasi";
        }
        result.push({
          id: item[0],
          nama: nama,
          status: status
        });
      }
      this.list_saran = result;
    });
  },
  methods: {
    selectSaran(item) {
      this.formSaran.lokasi = item.nama;
      this.formSaran.id = item.id;
      this.formSaran.status = item.status;
    },
    onSubmitTerimaSaran(e) {
      e.preventDefault();
      this.$refs["loading"].show();
      axios.post(host+'/terima-saran',{
        body: {
          id: this.formSaran.id
        }
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
    },
    onSubmitTolakSaran(e) {
      e.preventDefault();
      alert("akan menolak " + this.formSaran.id + ". fitur sedang dalam pengembangan ");
    }
  }
};
</script>

<style>
</style>
