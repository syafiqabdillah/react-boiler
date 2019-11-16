<template>
  <div class="animated fadeIn">
    <div>
      <b-row align-h="center">
        <b-col sm="12" lg="12">
          <b-card overlay img-src="img/relawan2.jpg"></b-card>
        </b-col>
      </b-row>
      <b-row align-h="center">
        <b-col sm="12" lg="12">
          <b-card overlay img-src="img/poor2.jpg"></b-card>
        </b-col>
      </b-row>
      <b-row align-h="center">
        <b-col sm="12" lg="12">
          <b-card overlay img-src="img/siap_makan.jpg"></b-card>
        </b-col>
      </b-row>
      <b-row align-h="center">
        <b-col sm="12" lg="8">
          <b-card title="GALERI KEGIATAN" align="center">
            <Carousel />
          </b-card>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
import Carousel from "../home/Carousel";
export default {
  components: {
    Carousel
  },
  data() {
    return {
      slide: 0,
      sliding: null
    };
  },
  methods: {
    onSlideStart() {
      this.sliding = true;
    },
    onSlideEnd() {
      this.sliding = false;
    }
  }
};
</script>

<style>
</style>
