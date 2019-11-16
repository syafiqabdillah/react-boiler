<template>
  <b-card>
    <b-list-group>
      <b-list-group-item>
        <div align="center">
          <h5>SUDAH MELAKUKAN DONASI ?</h5>
          <h5>AYO KLAIM DONASIMU !</h5>
        </div>
        <b-button @click="onClickKlaim" style="color:white;background-color:#B17563;border:0px;" block>KLAIM DONASI</b-button>
      </b-list-group-item>
      <b-list-group-item>
        <div align="center">
          <h5>MENGAPA MELAKUKAN KLAIM ?</h5>
        </div>
        <ul>
          <li><p>Donasi yang kami terima tanpa klaim akan dianggap berasal dari anonim</p></li>
          <li><p>Apabila kamu melakukan klaim, kami akan mengetahui bahwa donasi tersebut berasal dari kamu sehingga kami dapat mengenang kebaikanmu :)</p></li>
          <li><p>Kamu dapat melihat riwayat donasi yang pernah kamu lakukan</p></li>
        </ul>
      </b-list-group-item>
    </b-list-group>
  </b-card>
</template>

<script>
export default {
  methods:{
    onClickKlaim(){
      if(sessionStorage.getItem("id") === null){
        alert("Silahkan login terlebih dahulu sebelum melakukan klaim");
        this.$bvModal.show("login");
      } else {
        this.$bvModal.show("klaim");
      }
    }
  }
};
</script>

<style>
</style>

