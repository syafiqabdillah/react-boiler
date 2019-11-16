<template>
  <div class="animated fadeIn">
    <div align="center">DONASI SAYA</div>
    <b-tabs content-class="mt-3" fill>
      <b-tab title="Semua" active>
        <b-table
          id="table-donasi"
          :fields="field_donasi"
          :items="list_donasi"
          :per-page="perPage"
          :current-page="currentPage"
          sort-by="nominal"
          sort-desc
          responsive
          show-empty
        ></b-table>
        <b-pagination
          v-model="currentPage"
          :per-page="perPage"
          :total-rows="rows"
          aria-controls="table-donasi"
        ></b-pagination>
      </b-tab>
      <b-tab title="Terverifikasi">
        <b-table
          id="table-donasi-terverifikasi"
          :fields="field_donasi_khusus"
          :items="list_donasi_terverifikasi"
          :per-page="perPage"
          :current-page="currentPageTerverifikasi"
          responsive
          show-empty
        ></b-table>
        <b-pagination
          v-model="currentPageTerverifikasi"
          :per-page="perPage"
          :total-rows="rowsTerverifikasi"
          aria-controls="table-donasi_terverifikasi"
        ></b-pagination>
      </b-tab>
      <b-tab title="Diproses">
        <b-table
          id="table-donasi-diproses"
          :fields="field_donasi_khusus"
          :items="list_donasi_diproses"
          :per-page="perPage"
          :current-page="currentPageDiproses"
          responsive
          show-empty
        ></b-table>
        <b-pagination
          v-model="currentPageDiproses"
          :per-page="perPage"
          :total-rows="rowsDiproses"
          aria-controls="table-donasi"
        ></b-pagination>
      </b-tab>
    </b-tabs>
  </div>
</template>

<script>
import axios from "axios";
const host = process.env.VUE_APP_HOST;

export default {
  data() {
    return {
      list_donasi: [],
      list_donasi_diproses: [],
      list_donasi_terverifikasi: [],
      field_donasi: [
        { key: "diajukan", sortable: true },
        { key: "nominal", sortable: true, align:"right"},
        "kode",
        { key: "status", sortable: true }
      ],
      field_donasi_khusus: [
        { key: "diajukan", sortable: true },
        { key: "nominal", sortable: true },
        "kode"
      ],
      currentPage: 1,
      currentPageDiproses: 1,
      currentPageTerverifikasi: 1,
      perPage: 4
    };
  },
  computed: {
    rows() {
      return this.list_donasi.length;
    },
    rowsTerverifikasi() {
      return this.list_donasi_terverifikasi.length;
    },
    rowsDiproses() {
      return this.list_donasi_diproses.length;
    }
  },
  created() {
    axios
      .get(host + "/donasi/" + sessionStorage.getItem("id"))
      .then(response => {
        var newlist = [];
        var newlistdiproses = [];
        var newlistterverifikasi = [];
        for (var item in response.data.data) {
          var itemobj = response.data.data[item];
          var ts = new Date(itemobj[2]);
          itemobj[2] = ts.toDateString();
          itemobj[6] = 'Rp ' + itemobj[6].toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,');
          let status;
          if (itemobj[4] == 1) {
            status = "Diproses";
            newlistdiproses.push({
              diajukan: itemobj[2],
              nominal: itemobj[6],
              kode: "KLAIM-" + itemobj[1] + "-" + itemobj[3]
            });
          } else {
            status = "Terverifikasi";
            newlistterverifikasi.push({
              diajukan: itemobj[2],
              nominal: itemobj[6],
              kode: "KLAIM-" + itemobj[1] + "-" + itemobj[3]
            });
          }
          newlist.push({
            diajukan: itemobj[2],
            status: status,
            nominal: itemobj[6],
            kode: "KLAIM-" + itemobj[1] + "-" + itemobj[3]
          });
        }
        this.list_donasi_diproses = newlistdiproses;
        this.list_donasi = newlist;
        this.list_donasi_terverifikasi = newlistterverifikasi;
      })
      .catch(e => {
        alert(e);
      });
  }
};
</script>

<style>
</style>
