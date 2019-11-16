<template>
  <b-card>
    <b-table
      id="my-table-rekening"
      show-empty
      :items="items_rekening"
      :fields="fields_rekening"
      :current-page="currentPage"
      :per-page="perPage"
      responsive
    >
      <template slot="actions" slot-scope="row">
        <b-button
          size="sm"
          v-b-modal.edit-rekening
          placement="rightbottom"
          variant="primary"
          @click="changeSaldo(row.item)"
        >EDIT</b-button>
      </template>
    </b-table>
    <b-row>
      <b-col sm="6" lg="6">
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          aria-controls="my-table-rekening"
          :per-page="perPage"
        ></b-pagination>
      </b-col>
      <b-col sm="6" lg="6">
        <ModalRekening />
      </b-col>
    </b-row>

    <b-modal title="Edit Rekening" id="edit-rekening" @ok="onSubmitEdit">
      <b-form>
        <b-form-group
          id="input-group-edit-rekening-1"
          label="Nama Bank"
          label-for="input-edit-rekening-1"
        >
          <b-form-input
            id="input-eidt-rekening-1"
            v-model="formEdit.nama"
            type="text"
            disabled
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-edit-rekening-2"
          label="Saldo"
          label-for="input-edit-rekening-2"
        >
          <b-form-input id="input-eidt-rekening-2" v-model="formEdit.saldo" type="number" required></b-form-input>
        </b-form-group>
      </b-form>
    </b-modal>
    <b-modal ref="loading" hide-header hide-footer>
      <div align="center">
        <b-spinner label="Spinning"></b-spinner>
        <br />Loading...
      </div>
    </b-modal>
  </b-card>
</template>
<script>
import ModalRekening from "./ModalRekening";
import axios from "axios";
const host = process.env.VUE_APP_HOST;
export default {
  created() {
    axios.get(host + "/rekening").then(res => {
      this.items_rekening = res.data.data;
    });
  },
  name: "TableRekening",
  components: {
    ModalRekening
  },
  data() {
    return {
      items_rekening: [],
      fields_rekening: [
        { key: "1", label: "Nama" },
        { key: "2", label: "Atas Nama" },
        { key: "3", label: "No Rekening" },
        { key: "4", label: "Saldo" },
        "actions"
      ],
      formEdit: {
        id: null,
        saldo: null,
        nama: ""
      },
      currentPage: 1,
      perPage: 5
    };
  },
  computed: {
    rows() {
      return this.items_rekening.length;
    }
  },
  methods: {
    changeSaldo(row) {
      this.formEdit.saldo = row[4];
      this.formEdit.id = row[0];
      this.formEdit.nama = row[1];
      console.log(this.formEdit);
    },
    onSubmitEdit(e) {
      e.preventDefault();
      //alert(JSON.stringify(this.formEdit));
      this.$refs["loading"].show();
      axios
        .post(host + "/edit-saldo", {
          body: this.formEdit
        })
        .then(res => {
          this.$refs["loading"].hide();
          alert("Perubahan berhasil dilakukan");
          window.location.reload();
        })
        .catch(e => {
          this.$refs["loading"].hide();
          alert(e);
          window.location.reload();
        });
    }
  }
};
</script>

