<template>
  <div class="animated fadeIn">
    <b-row>
      <b-col sm="12" lg="12">
        <b-card overlay img-src="img/relawan1.jpg">
          <b-row align-h="center" align-v="end">
            <div style="bottom:20px;position:absolute;">
              <!-- <b-button
                to="/donasi"
                style="background-color:#FFA63D;color:black;border:0px;"
                size="lg"
                id="button-donasi"
                v-if="!isMobile"
              >DONASI SEKARANG</b-button>
              <b-button
                to="/donasi"
                style="background-color:#FFA63D;color:black;border:0px;"
                size="sm"
                id="button-donasi"
                v-if="isMobile"
              >DONASI SEKARANG</b-button> -->
            </div>
          </b-row>
        </b-card>
      </b-col>
    </b-row>
    <b-row>
      <b-col sm="6" lg="6">
        <b-card>
          <b-card-text>
            <div align="center">
              <span>
                <strong>DONASI TERKUMPUL</strong>
              </span>
              <br />
              <span>
                <strong>
                  <h1>Rp {{ totalDonasi }}</h1>
                </strong>
              </span>
              <span>DARI PARA DONATUR</span>
              <br />
            </div>
          </b-card-text>
        </b-card>
      </b-col>
      <b-col sm="6" lg="6">
        <b-card>
          <b-card-text>
            <div align="center">
              <span>
                <strong>CAMPAIGN DILAKUKAN</strong>
              </span>
              <br />
              <span>
                <strong>
                  <a href="#" v-b-modal.list-campaign>
                    <h1>{{ total_campaign }}x</h1>
                  </a>
                </strong>
              </span>
              <span>DI BERBAGAI DAERAH</span>
              <br />
            </div>
          </b-card-text>
        </b-card>
      </b-col>
    </b-row>
    <b-row>
      <b-col sm="12" lg="12">
        <b-card title="APA ITU OASIS?" bg-variant="dark" text-variant="white">
          <b-card-text>
            <span>
              Oasis adalah aplikasi berbasis web yang yang memudahkan masyarakat dalam
              <u>berbagi makanan</u> dengan masyarakat lain yang lebih membutuhkan.
            </span>
          </b-card-text>
          <b-button
            to="/tentang"
            style="background-color:#B17563;color:white;border:0px;"
          >KENALI KAMI LEBIH LANJUT</b-button>
        </b-card>
      </b-col>
    </b-row>

    <b-row>
      <b-col sm="6" lg="6">
        <b-card title="GALERI KEGIATAN" align="center">
          <Carousel />
        </b-card>
      </b-col>
      <b-col sm="6" lg="6">
        <b-card title="PESAN DONATUR" align="center">
          <Testimoni />
        </b-card>
      </b-col>
    </b-row>
    <b-modal id="list-campaign" title="Tanggal dan Lokasi Campaign" centered hide-footer>
      <b-list-group>
        <b-list-group-item
          v-bind:key="campaign.id"
          v-for="campaign in list_campaign"
          align="left"
        >{{ campaign[2]}} - {{campaign[3]}} - Target ,<strong> {{campaign[4]}} </strong> Kotak - Realisasi <strong> {{campaign[1]}} </strong> Kotak</b-list-group-item>
      </b-list-group>
    </b-modal>
  </div>
</template>

<script>
import Carousel from "./Carousel";
import Testimoni from "./Testimoni";
import axios from "axios";
const host = process.env.VUE_APP_HOST;

export default {
  created() {
    axios.get(host + "/total-donasi").then(res => {
      this.totalDonasi = res.data.data[0][0].toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,');
    });
    axios.get(host + "/all-pembagian").then(res => {
      this.total_campaign = res.data.data.length;
      for (var i in res.data.data) {
        res.data.data[i][2] = new Date(res.data.data[i][2]).toDateString();
      }
      this.list_campaign = res.data.data;
    });
  },
  name: "home",
  components: {
    Carousel,
    Testimoni
  },
  data: function() {
    return {
      totalDonasi: 0,
      total_campaign: 0,
      list_campaign: []
    };
  },
  computed: {
    isMobile() {
      return window.matchMedia("only screen and (max-width: 760px)").matches;
    }
  }
};
</script>

<style>
</style>
