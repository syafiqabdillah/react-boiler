<template>
  <div class="animated fadeIn" style="padding:15px">
    <b-row align-h="center">
      <b-col sm="6" lg="3">
        <b-card>
          <b-row align-h="center">
            <b-img height="100px" src="img/icon.png" rounded="circle"></b-img>
          </b-row>
          <b-row align-h="center">
            <h5>{{ nama }}</h5>
          </b-row>
          <b-row align-h="center">
            <b-col lg="12" sm="12">
              <b-list-group>
                <b-list-group-item @click="donasiSaya" href="#">
                  <h6>Donasi Saya</h6>
                </b-list-group-item>
                <b-list-group-item @click="saranSaya" href="#">
                  <h6>Saran Lokasi Saya</h6>
                </b-list-group-item>
                <!-- <b-list-group-item @click="bergabungSaya" href="#">
                  <h6>Riwayat Bergabung</h6>
                </b-list-group-item> -->
              </b-list-group>
              <br />
              <b-list-group>
                <b-list-group-item @click="removeSession" href="#">
                  <h6>Logout</h6>
                </b-list-group-item>
              </b-list-group>
            </b-col>
          </b-row>
        </b-card>
      </b-col>
      <b-col sm="12" lg="6">
        <b-card>
          <b-row>
            <b-col lg="12" sm="12">
              <div align="center">
                <h6>
                  TOTAL DONASI SAYA
                </h6>
                <h2>Rp {{ total_donasi }}</h2>
              </div>
            </b-col>
            <!-- <b-col lg="6" sm="6">
              <div align="center">
                <h6>TOTAL SAYA IKUT BERGABUNG SEBAGAI RELAWAN</h6>
                <h2>4X</h2>
              </div>
            </b-col> -->
          </b-row>
          <hr />
          <b-row>
            <b-col lg="12" sm="12">
              <keep-alive>
                <component v-bind:is="changeIsi"></component>
              </keep-alive>
            </b-col>
          </b-row>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import DonasiSaya from "./DonasiSaya";
import SaranSaya from "./SaranSaya";
import BergabungSaya from "./BergabungSaya";
import axios from "axios";
const host = process.env.VUE_APP_HOST;

export default {
  beforeMount() {
    if (sessionStorage.getItem("id") === null) {
      window.location.href = "#/home";
    }
  },
  data() {
    return {
      route_now: 0,
      total_donasi: 0
    };
  },
  created() {
    axios
      .get(host + "/donasi/" + sessionStorage.getItem("id"))
      .then(response => {
        var total = 0;
        for (var item in response.data.data) {
          var itemobj = response.data.data[item];
          if (itemobj[4] == 2) {
            total += itemobj[6];
          }
        }
        this.total_donasi = total.toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,');
      })
      .catch(e => {
        alert(e);
      });
  },
  components: {
    DonasiSaya,
    SaranSaya,
    BergabungSaya
  },
  methods: {
    donasiSaya() {
      this.route_now = 0;
    },
    saranSaya() {
      this.route_now = 1;
    },
    bergabungSaya() {
      this.route_now = 2;
    },
    removeSession() {
      sessionStorage.clear();
      window.location.href = "/home";
    }
  },
  computed: {
    changeIsi() {
      if (this.route_now == 0) {
        return DonasiSaya;
      } else if (this.route_now == 1) {
        return SaranSaya;
      } else if (this.route_now == 2) {
        return BergabungSaya;
      }
    },
    nama() {
      return sessionStorage.getItem("nama");
    }
  }
};
</script>

<style>
</style>
