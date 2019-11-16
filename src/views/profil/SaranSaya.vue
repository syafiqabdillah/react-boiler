<template>
  <div class="animated fadeIn">
    <div align="center">SARAN SAYA</div>
    <b-table
      :per-page="perPage"
      :current-page="currentPage"
      id="table-saran"
      :fields="list_field"
      :items="list_saran"
      responsive
      show-empty
    ></b-table>
    <b-pagination
      v-model="currentPage"
      :per-page="perPage"
      :total-rows="rows"
      aria-controls="table-saran"
    ></b-pagination>
  </div>
</template>

<script>
import axios from "axios";
const host = process.env.VUE_APP_HOST;

export default {
  data() {
    return {
      list_saran: [],
      list_field: ["lokasi", { key: "tanggal_diajukan", sortable: true }],
      currentPage: 1,
      perPage: 4 
    };
  },
  created() {
    axios
      .get(host + "/saran-lokasi/" + sessionStorage.getItem("id"))
      .then(res => {
        var lst = res.data.data;
        var new_list = [];
        for (var ind in lst) {
          var newdate = new Date(lst[ind][8]);
          var newdate = newdate.toDateString();
          var item = lst[ind];
          var newitem = {
            lokasi:
              item[5] +
              " - " +
              item[4] +
              " - " +
              item[3] +
              " - " +
              item[2],
            tanggal_diajukan: newdate
          };
          new_list.push(newitem);
        }
        this.list_saran = new_list;
      })
      .catch(e => {});
  },
  computed: {
    rows() {
      return this.list_saran.length;
    }
  }
};
</script>

<style>
</style>
