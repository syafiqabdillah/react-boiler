<template>
  <div class="animated fadeIn" style="padding:15px">
    <b-row align-h="center">
      <b-col sm="12" lg="6">
        <div align="center">
          <h2>Kelola Klaim</h2>
        </div>
        <TableKlaim />
      </b-col>
      <b-col sm="12" lg="6">
        <div align="center">
          <h2>Kelola Pengguna</h2>
        </div>
        <TableUser />
      </b-col>
    </b-row>
    <b-row>
      <b-col sm="12" lg="6">
        <div align="center">
          <h2>Kelola Campaign</h2>
        </div>
        <TableLaporan />
      </b-col>
      <b-col sm="12" lg="6">
        <div align="center">
          <h2>Kelola Saran Lokasi Campaign</h2>
        </div>
        <TableSaran />
      </b-col>
    </b-row>
    <b-row>
      <b-col sm="12" lg="6">
        <div align="center">
          <h2>Konfigurasi Sistem</h2>
        </div>
        <Konfigurasi />
      </b-col>
      <b-col sm="12" lg="6">
        <div align="center">
          <h2>Kelola Rekening</h2>
        </div>
        <TableRekening />
      </b-col>
    </b-row>
  </div>
</template>
<script>
import TableLaporan from "./TableLaporan";
import TableRekening from "./TableRekening";
import TableSaran from "./TableSaran";
import TableKlaim from "./TableKlaim";
import TableUser from "./TableUser";
import Konfigurasi from "./Konfigurasi";
export default {
  beforeCreate() {
    if (sessionStorage.getItem("role") != "admin") {
      window.location.href = "/";
    }
  },
  components: {
    TableLaporan,
    TableRekening,
    TableKlaim,
    TableUser,
    Konfigurasi,
    TableSaran
  }
};
</script>
<style>
</style>


