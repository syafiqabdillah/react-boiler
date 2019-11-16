<template>
  <div class="animated fadeIn" style="margin:15px">
    <div align="center">
      <h2>SEKILAS TENTANG DONASI</h2>
      <p>Donasi yang berhasil dikumpulkan oleh donatur melalui Oasis akan dibagikan dalam bentuk makanan kepada masyarakat yang membutuhkan (Rp 15.000 / makanan)</p>
    </div>
    <b-row align-h="center">
      <b-col lg="6" sm="12">
        <SekilasKiri />
      </b-col>
      <b-col lg="6" sm="12">
        <SekilasKanan />
      </b-col>
    </b-row>

    <div align="center" id="kirim-donasi">
      <h2>KIRIM DONASI</h2>
      <p>Untuk melakukan donasi, kamu hanya perlu melakukan transfer ke salah satu rekening berikut</p>
    </div>
    <b-row align-h="center">
      <b-col lg="4" sm="12">
        <ListRekening />
      </b-col>
      <b-col lg="8" sm="12">
        <KlaimDonasi />
      </b-col>
    </b-row>
  </div>
</template>

<script>
import ListRekening from "./ListRekening";
import KlaimDonasi from "./KlaimDonasi";
import SekilasKiri from "./SekilasKiri";
import SekilasKanan from "./SekilasKanan";
// 737748629373-e94koqv6th1nrtjnahlalm3p3spvsbb8.apps.googleusercontent.com
// __TPbEr6PqTrp70TkFOq_etu
export default {
  name: "donasi",
  components: {
    ListRekening,
    KlaimDonasi,
    SekilasKiri,
    SekilasKanan
  }
};
</script>

<style>
html {
  scroll-behavior: smooth;
}
</style>

