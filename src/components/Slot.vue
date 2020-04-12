<template>
  <v-card class="d-flex" hover v-on:mousedown="down()" v-on:mouseup="up()">
    <v-img
      transition="slide-y-transition"
      src="@/assets/ducker.png"
      aspect-ratio="1"
      class="grey lighten-2"
      v-if="target && !hit"
    ></v-img>
    <v-img
      transition="slide-y-transition"
      src="@/assets/whacked.png"
      aspect-ratio="1"
      class="grey lighten-2"
      v-if="target && hit"
    ></v-img>
    <v-img
      transition="slide-y-transition"
      src="@/assets/whack.png"
      aspect-ratio="1"
      class="grey lighten-2"
      v-show="!target"
    ></v-img>
  </v-card>
</template>

<script>
export default {
  props: {
    target: Boolean,
    index: Number
  },
  data() {
    return {
      hit: false,
      timer: null,
    };
  },
  methods: {
    down() {
      this.hit = true;
    },
    up() {
      this.hit = false;
      this.clearTimer();
      this.$emit("whacked", this.index);
    },
    clearTimer() {
      if(this.timer !== null) {
        console.log("cleared");
        clearTimeout(this.timer);
        this.timer=null;
      }
    },
    setupTimeout() {
      this.timer = setTimeout(() => {
        console.log("failed");
        this.$emit("lose");
      }, 1000); 
    }
  },
  watch: {
    target(value) {
      if(value) {
        this.setupTimeout();
      }
    }
  },
};
</script>

<style scoped></style>
