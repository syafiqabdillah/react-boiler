<template>
  <div>
    <b-card>
      <b-table
        id="table-klaim"
        :fields="fieldKlaim"
        :items="listKlaim"
        :per-page="perPage"
        :current-page="currentPage"
        responsive
        show-empty
      >
        <template slot="actions" slot-scope="row">
          <b-button
            size="sm"
            v-b-tooltip.hover
            title="Klik setelah cek bukti transfer yang dikirim ke email admin"
            placement="rightbottom"
            v-b-modal.verifikasi
            @click="verify(row.item.kode)"
            variant="primary"
          >VERIFIKASI KLAIM</b-button>
        </template>
      </b-table>
      <b-pagination
        v-model="currentPage"
        :per-page="perPage"
        :total-rows="rows"
        aria-controls="table-klaim"
      ></b-pagination>
    </b-card>
    <b-modal
      title="Verifikasi Klaim"
      id="verifikasi"
      @shown="focusMyElement"
      @cancel="onResetVerify"
      @ok="onSubmitVerify"
    >
      <b-form @keypress.enter="onSubmitVerify">
        <b-form-group id="input-group-verify-2" label="Kode" label-for="input-verify-2">
          <b-form-input
            id="input-verify-2"
            v-model="formVerifikasi.kode"
            type="text"
            required
            disabled
          ></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-verify-1"
          label="Nominal pada bukti transfer"
          label-for="input-verify-1"
        >
          <b-form-input
            id="input-verify-1"
            v-model="formVerifikasi.nominal"
            type="number"
            required
            ref="focusThis"
          ></b-form-input>
        </b-form-group>
      </b-form>
    </b-modal>
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
      listKlaim: [],
      fieldKlaim: ["kode", { key: "actions", label: "Actions" }],
      currentPage: 1,
      perPage: 5,
      formVerifikasi: {
        kode: "",
        nominal: null
      }
    };
  },
  created() {
    axios.get(host + "/klaim-diproses").then(res => {
      var list_klaim_rapi = [];
      for (var ind in res.data.data) {
        var klaim = res.data.data[ind];
        var kode = "KLAIM-" + klaim[1] + "-" + klaim[3];
        list_klaim_rapi.push({ kode: kode });
      }
      this.listKlaim = list_klaim_rapi;
    });
  },
  computed: {
    rows() {
      return this.listKlaim.length;
    }
  },
  methods: {
    verify(kode) {
      this.formVerifikasi.kode = kode;
    },
    onSubmitVerify(e) {
      e.preventDefault();
      if (this.formVerifikasi.nominal === null) {
        alert("Mohon lengkapi form verifikasi");
      } else {
        this.$refs["loading"].show();
        var data = this.formVerifikasi.kode + "@" + this.formVerifikasi.nominal;
        axios
          .post(host + "/verifikasi-klaim/" + data)
          .then(res => {
            this.$refs["loading"].hide();
            if (!alert("Verifikasi klaim berhasil")) {
              window.location.reload();
            }
          })
          .catch(e => {
            this.$refs["loading"].hide();
            alert(e);
          });
      }
    },
    onResetVerify() {
      (this.formVerifikasi.kode = ""), (this.formVerifikasi.nominal = null);
    },
    focusMyElement() {
      this.$refs.focusThis.focus();
    }
  }
};
</script>

<style>
</style>
