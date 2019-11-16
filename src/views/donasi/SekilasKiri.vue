<template>
  <div>
    <b-card title="DONASI MASUK" align="center">
      <b-row>
        <b-col lg="6" sm="6">
          <b-list-group>
            <b-list-group-item>
              <div align="center">
                <h5>TERBARU</h5>
              </div>
            </b-list-group-item>
            <b-list-group-item v-bind:key="item.id" v-for="item in list_terbaru" align="left">
              <span>
                {{item.nama[0]}}
                (Rp {{item.nominal}})
              </span>
            </b-list-group-item>
          </b-list-group>
        </b-col>
        <b-col lg="6" sm="6">
          <b-list-group>
            <b-list-group-item>
              <div align="center">
                <h5>TERTINGGI</h5>
              </div>
            </b-list-group-item>
            <b-list-group-item v-bind:key="item.id" v-for="item in list_tertinggi" align="left">
              <span>
                {{item.nama[0]}}
                (Rp {{item.nominal}})
              </span>
            </b-list-group-item>
          </b-list-group>
        </b-col>
      </b-row>
      <span align="right">
        <br>
        <p style="color:grey">Terakhir diupdate {{ terakhir_update }}</p>
      </span>
    </b-card>
  </div>
</template>

<script>
import axios from 'axios';
const host = process.env.VUE_APP_HOST;

export default {
  data(){
    return{
      list_terbaru:[],
      list_tertinggi:[],
      terakhir_update: null
    }
  },
  created(){
    axios.get(host + '/sekilas-donasi')
    .then(res=>{ 
      this.list_terbaru = res.data.data['terbaru']
      this.list_tertinggi = res.data.data['tertinggi']
    })

    axios.get(host+'/terakhir-update')
    .then(res=>{
      this.terakhir_update = new Date(res.data.data[0][0])
    })
    .catch(e=>{
      alert(e);
    })
  }
};
</script>

<style>
</style>
