<template>
  <div class="app">
    <ModalRegister />
    <ModalLogin />
    <ModalKlaim />
    <ModalSaran />
    <AppHeader
      fixed
      style="box-shadow:0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19)"
    >
      <b-link class="navbar-brand" to="/">
        <img
          class="navbar-brand-full"
          src="img/logo_navbar.png"
          width="89"
          height="90"
          alt="CoreUI Logo"
        />
        <img
          class="navbar-brand-minimized"
          src="img/icon.png"
          width="30"
          height="30"
          alt="CoreUI Logo"
        />
      </b-link>
      <b-navbar-nav class="d-md-down-none">
        <b-nav-item class="px-3" to="/home">
          <div style="color:#FFA63D;font-weight:bold">BERANDA</div>
        </b-nav-item>
        <b-nav-item class="px-3" to="/tentang">
          <div style="color:#FFA63D;font-weight:bold">TENTANG KAMI</div>
        </b-nav-item>
        <b-nav-item class="px-3" to="/donasi">
          <div style="color:#FFA63D;font-weight:bold">DONASI</div>
        </b-nav-item>
        <b-nav-item class="d-md-down-none px-3" to="/bergabung">
          <div style="color:#FFA63D;font-weight:bold">MENJADI RELAWAN</div>
        </b-nav-item>
      </b-navbar-nav>
      <b-navbar-nav class="ml-auto">
        <b-nav-item v-if="isNotLoggedIn" class="d-md-down-none px-3">
          <b-button
            v-b-modal.register
            pill
            style="background-color:#FFA63D;color:white;border:0px;font-weight:bold"
          >REGISTER</b-button>
        </b-nav-item>
        <b-nav-item v-if="isNotLoggedIn" class="d-md-down-none px-3">
          <b-button
            v-b-modal.login
            pill
            style="background-color:white;color:#FFA63D;border:0px;font-weight:bold"
          >LOGIN</b-button>
        </b-nav-item>
        <b-nav-item v-if="!isNotLoggedIn && isAdmin" to="/admin" class="d-md-down-none px-3">
          <b-button
            pill
            style="background-color:white;color:#FFA63D;border:0px;font-weight:bold"
          >ADMIN</b-button>
        </b-nav-item>
        <b-nav-item v-if="!isNotLoggedIn" class="d-md-down-none px-3">
          <b-button
            v-b-modal.saran
            pill
            style="background-color:white;color:#FFA63D;border:0px;font-weight:bold"
          >SARAN LOKASI</b-button>
        </b-nav-item>
        <b-nav-item v-if="!isNotLoggedIn" class="d-md-down-none px-3">
          <b-button
            v-b-modal.klaim
            pill
            style="background-color:white;color:#FFA63D;border:0px;font-weight:bold"
          >KLAIM DONASI</b-button>
        </b-nav-item>
        <b-nav-item v-if="!isNotLoggedIn" class="d-md-down-none px-3" to="/profil">
          <span style="color:black;font-weight:bold">Selamat datang, {{ nama }}</span>
        </b-nav-item>
        <DefaultHeaderDropdownAccnt />
      </b-navbar-nav>
    </AppHeader>
    <div class="app-body">
      <main class="main">
        <div class="container-fluid" style="padding:0px">
          <router-view></router-view>
        </div>
      </main>
    </div>
    <TheFooter style="background-color:black">
      <div>
        <span class="ml-1" style="color:white">&copy; Copyright 2019 Oasis, All Rights Reserved</span>
      </div>
    </TheFooter>
  </div>
</template>

<script>
import nav from "@/_nav";
import {
  Header as AppHeader,
  Footer as TheFooter,
  Breadcrumb
} from "@coreui/vue";
import DefaultHeaderDropdownAccnt from "./DefaultHeaderDropdownAccnt";
import ModalRegister from "./ModalRegister";
import ModalLogin from "./ModalLogin";
import ModalKlaim from "./ModalKlaim";
import ModalSaran from "./ModalSaran";

export default {
  beforeCreate() {
    if (location.protocol != "https:" && process.env.NODE_ENV == "production") {
      location.href =
        "https:" +
        window.location.href.substring(window.location.protocol.length);
    }
  },
  name: "DefaultContainer",
  components: {
    AppHeader,
    TheFooter,
    Breadcrumb,
    DefaultHeaderDropdownAccnt,
    ModalRegister,
    ModalLogin,
    ModalKlaim,
    ModalSaran
  },
  data() {
    return {
      nav: nav.items
    };
  },
  computed: {
    nama() {
      return sessionStorage.getItem("nama");
    },
    name() {
      return this.$route.name;
    },
    list() {
      return this.$route.matched.filter(
        route => route.name || route.meta.label
      );
    },
    isNotLoggedIn() {
      return sessionStorage.getItem("id") === null;
    },
    isAdmin(){
      return sessionStorage.getItem("role") === "admin";
    }
  },
  methods: {
    removeSession() {
      sessionStorage.clear();
      window.location.href = "/home";
    }
  }
};
</script>
