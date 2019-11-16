<template>
  <AppHeaderDropdown right no-caret>
    <template slot="header">
      <img
        src="img/list.svg"
      class="img-avatar"/>
    </template>\
    <template slot="dropdown">
      <b-dropdown-header tag="div" class="text-center">
        <strong>{{ nama }}</strong>
      </b-dropdown-header>
      <b-dropdown-item to="/profil"  v-if="!isNotLoggedIn">
        <i class="fa fa-shield" /> Dashboard
      </b-dropdown-item>
      <b-dropdown-divider v-if="!isNotLoggedIn"></b-dropdown-divider>
      <b-dropdown-item to="/home">
        <i class="fa fa-shield" /> Beranda
      </b-dropdown-item>
      <b-dropdown-item to="/tentang">
        <i class="fa fa-shield" /> Tentang Kami
      </b-dropdown-item>
      <b-dropdown-item to="/donasi">
        <i class="fa fa-shield" /> Donasi
      </b-dropdown-item>
      <b-dropdown-item to="/bergabung">
        <i class="fa fa-shield" /> Menjadi Relawan
      </b-dropdown-item>
      <b-dropdown-divider></b-dropdown-divider>
      <b-dropdown-item v-if="isNotLoggedIn" v-b-modal.register>
        <i class="fa fa-shield" /> Register
      </b-dropdown-item>
      <b-dropdown-item v-if="isNotLoggedIn" v-b-modal.login>
        <i class="fa fa-lock" /> Login
      </b-dropdown-item>
      <b-dropdown-item v-if="!isNotLoggedIn && isAdmin" to="/admin">
        <i class="fa fa-shield" /> Admin
      </b-dropdown-item>
      <b-dropdown-item v-if="!isNotLoggedIn" v-b-modal.saran>
        <i class="fa fa-shield" /> Saran Lokasi
      </b-dropdown-item>
      <b-dropdown-item v-if="!isNotLoggedIn" v-b-modal.klaim>
        <i class="fa fa-shield" /> Klaim Donasi
      </b-dropdown-item>
      <b-dropdown-divider v-if="!isNotLoggedIn"></b-dropdown-divider>
      <b-dropdown-item v-if="!isNotLoggedIn" @click="removeSession">
        <i class="fa fa-lock" /> Logout
      </b-dropdown-item>
    </template>
  </AppHeaderDropdown>
</template>

<script>
import { HeaderDropdown as AppHeaderDropdown } from "@coreui/vue";
export default {
  name: "DefaultHeaderDropdownAccnt",
  components: {
    AppHeaderDropdown
  },
  data: () => {
    return { itemsCount: 42 };
  },
  computed: {
    nama(){
      if (sessionStorage.getItem("id") !== null){
        return sessionStorage.getItem("nama");
      } else {
        return "Menu";
      }
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
