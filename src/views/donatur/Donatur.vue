<template>
  <div class="animated fadeIn" style="padding:15px">
    <div align="center">
      <h2>List Donatur</h2>
      <b-row align-h="center">
        <b-col lg="6" sm="12">
          <b-card>
            <b-table :items="donatur" :fields="field" striped></b-table>
          </b-card>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
import axios from "axios";
const host = process.env.VUE_APP_HOST;
export default {
  data() {
    return {
      donatur: [],
      field: ["nama", "username"]
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
  }
};
</script>

<style>
</style>
