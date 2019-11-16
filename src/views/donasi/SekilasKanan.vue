<template>
  <div>
    <b-row>
      <b-col lg="12" sm="12">
        <b-card>
          <div align="center">
            <p>Yuk ikut berdonasi di Oasis dan atasi masalah kelaparan bersama kami</p>
            <b-button
              style="background-color:#B17563;color:white;border:0px;"
              block
              @click="scrollToKirimDonasi"
            >DONASI SEKARANG</b-button>
          </div>
        </b-card>
      </b-col>
    </b-row>
    <b-row>
      <b-col lg="12" sm="12">
        <b-card>
          <b-list-group>
            <b-list-group-item>
              <div align="center">
                <h5>KEUANGAN OASIS</h5>
              </div>
            </b-list-group-item>
            <b-list-group-item align="left">
              <b-row>
                <b-col lg="6" sm="6">
                  <div align="center">
                    <h6>SALDO OASIS SAAT INI</h6>
                    <h4>Rp {{ total_saldo }}</h4>
                  </div>
                </b-col>
                <b-col lg="6" sm="6">
                  <div align="center">
                    <h6>DONASI TERSALURKAN</h6>
                    <h4>Rp {{ donasi_tersalurkan }}</h4>
                  </div>
                </b-col>
              </b-row>
            </b-list-group-item>
          </b-list-group>
        </b-card>
      </b-col>
    </b-row>
    <b-row>
      <b-col lg="12" sm="12">
        <b-card>
          <b-list-group>
            <b-list-group-item>
              <div align="center">
                <h5>PENYALURAN MAKANAN</h5>
              </div>
            </b-list-group-item>
            <b-list-group-item align="left">
              <b-row>
                <b-col lg="6" sm="6">
                  <div align="center">
                    <h6>LOKASI CAMPAIGN</h6>
                    <a href="#" v-b-modal.list-campaign>
                      <h4>{{ total_campaign }} LOKASI</h4>
                    </a>
                  </div>
                </b-col>
                <b-col lg="6" sm="6">
                  <div align="center">
                    <h6>MAKANAN TERSALURKAN</h6>
                    <h4>{{ total_pembagian }} KOTAK</h4>
                  </div>
                </b-col>
              </b-row>
            </b-list-group-item>
          </b-list-group>
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
import axios from "axios";
const host = process.env.VUE_APP_HOST;

export default {
  methods: {
    scrollToKirimDonasi() {
      var element = document.getElementById("kirim-donasi");
      element.scrollIntoView();
    }
  },
  data() {
    return {
      total_saldo: 0,
      total_pembagian: 0,
      total_campaign: 0,
      list_campaign: []
    };
  },
  computed:{
    donasi_tersalurkan(){
      return (this.total_pembagian * 15000).toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,');
    }
  },
  created() {
    axios.get(host + "/rekening").then(res => {
      this.total_saldo = res.data.total_saldo_rekening.toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,');
    });

    axios.get(host + "/pembagian").then(res => {
      this.total_pembagian = res.data.data[0][0];
    });

    axios.get(host + "/all-pembagian").then(res => {
      this.total_campaign = res.data.data.length;
      for (var i in res.data.data) {
        res.data.data[i][2] = new Date(res.data.data[i][2]).toDateString();
      }
      this.list_campaign = res.data.data;
    });
  }
};
</script>

<style>
</style>
