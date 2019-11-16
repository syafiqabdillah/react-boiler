<template>
  <div>
    <b-carousel
      id="carousel-1"
      v-model="slide"
      :interval="2500"
      controls
      indicators
      img-height="30%"
      background="#ababab"
      style="text-shadow: 1px 1px 2px #333;"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >
      <b-carousel-slide img-src="img/poor1.jpg">
        <h1>Syafiq Abdillah U</h1>
        <h2>Mahasiswa</h2>
        <p style="color:black;background-color:white">Berkat Oasis, saya jadi bisa berbagi</p>
      </b-carousel-slide>
      <b-carousel-slide img-src="img/poor1.jpg">
        <h1>Syafiq Abdillah U</h1>
        <h2>Mahasiswa</h2>
        <p style="color:black;background-color:white">Berkat Oasis, saya jadi bisa berbagi</p>
      </b-carousel-slide>
    </b-carousel>
  </div>
</template>
<script>
export default {
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


