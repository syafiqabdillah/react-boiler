<template>
  <div>
    <b-carousel
      id="carousel-1"
      v-model="slide"
      :interval="2500"
      controls
      indicators
      background="#ababab"
      style="text-shadow: 1px 1px 2px #333;"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >
      <b-carousel-slide img-src="img/poor3.jpg">
      </b-carousel-slide>
      <b-carousel-slide img-src="img/team.jpg">
      </b-carousel-slide>
      <b-carousel-slide img-src="img/poor2_cropped.jpg">
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


