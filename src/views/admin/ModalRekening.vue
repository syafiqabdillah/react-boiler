<template>
  <div align="right">
    <b-button v-b-modal.modal-rekening variant="primary">Tambah Rekening</b-button>
    <b-modal
      id="modal-rekening"
      title="Tambah Rekening"
      @shown="focusMyElement"
      @cancel="onResetRekening"
      @ok="onSubmitRekening"
    >
      <b-form>
        <b-form-group id="input-group-rekening-1" label="Nama Bank" label-for="input-rekening-1">
          <b-form-input
            id="input-rekening-1"
            v-model="formRekening.nama"
            type="text"
            required
            ref="focusThis"
          ></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-rekening-2" label="Kode Bank" label-for="input-rekening-2">
          <b-form-input id="input-rekening-2" v-model="formRekening.kode_bank" type="text" required></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-rekening-3" label="No Rekening" label-for="input-rekening-3">
          <b-form-input
            id="input-rekening-3"
            v-model="formRekening.no_rekening"
            type="text"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group id="input-group-rekening-4" label="Saldo" label-for="input-rekening-4">
          <b-form-input id="input-rekening-4" v-model="formRekening.saldo" type="number" required></b-form-input>
        </b-form-group>
      </b-form>
    </b-modal>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formRekening: {
        nama: "",
        no_rekening: "",
        kode_bank: "",
        saldo: null
      }
    };
  },
  methods: {
    onSubmitRekening(e) {
      e.preventDefault();
      console.log(JSON.stringify(this.formRekening));
    },
    onResetRekening() {
      this.formRekening.nama = "";
      this.formRekening.no_rekening = "";
      this.formRekening.kode_bank = "";
      this.formRekening.saldo = null;
    },
    focusMyElement() {
      this.$refs.focusThis.focus();
    }
  }
};
</script>

<style>
</style>
