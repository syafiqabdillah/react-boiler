<template>
  <div>
    <b-card>
      <b-table
        :items="donatur"
        :fields="field"
        id="table-donatur"
        :per-page="perPage"
        :current-page="currentPage"
        responsive
        show-empty
      >
        <template slot="actions" slot-scope="row">
          <b-button @click="change(row.item.username)" size="sm" variant="primary" v-if="row.item.role !== 'admin'">JADIKAN ADMIN</b-button>
          <b-button @click="change(row.item.username)" size="sm" variant="secondary" v-if="row.item.role === 'admin'" disabled>JADIKAN ADMIN</b-button>
        </template>
      </b-table>
      <b-pagination
        v-model="currentPage"
        :per-page="perPage"
        :total-rows="rows"
        aria-controls="table-donatur"
      ></b-pagination>
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
  data() {
    return {
      donatur: [],
      currentPage: 1,
      perPage: 5,
      field: ["nama", "username", "role", "actions"]
    };
  },
  created() {
    axios
      .get(host + "/donatur")
      .then(response => {
        this.donatur = response.data.data.reverse();
      })
      .catch(e => {
        alert(e);
      });
  },
  methods: {
    change(username) {
      this.$refs["loading"].show();
      axios.post(host + '/make-admin',{
        body:{
          username:username
        }
      })
      .then(res=>{
        this.$refs["loading"].hide();
        alert('berhasil');
        window.location.reload();
      })  
      .catch(e=>{
        this.$refs["loading"].hide();
        alert(e);
        window.location.reload();
      })
    }
  },
  computed: {
    rows() {
      return this.donatur.length;
    }
  }
};
</script>

<style>
</style>
