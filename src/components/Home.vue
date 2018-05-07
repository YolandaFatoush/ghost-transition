<template>
  <div class="home" ref="hauntedHouse">
    <div class="square"></div>
    <div class="square-ghost" ref="ghost1"></div>
    <div class="square-ghost" ref="ghost2"></div>
    <div class="square-ghost" ref="ghost3"></div>
    <div class="rebel-ghost" ref="ghost4"></div>
    <button @click="animate">O</button>
  </div>
</template>

<script>
import anime from 'animejs';

export default {
  name: 'Home',
  data() {
    return {
      animating: false,
      pos: 1,
    };
  },
  methods: {
    animate() {
      if (this.animating) {
        console.log('still animating');
      } else {
        console.log('animation started');
        const parent = this.$refs.hauntedHouse.getBoundingClientRect();
        const child = this.$refs[`ghost${this.pos}`].getBoundingClientRect();
        this.pos = this.pos === 4 ? 1 : this.pos + 1;
        this.animating = true;
        child.height = child.height ? child.height : child.bottom - child.top;
        child.width = child.width ? child.width : child.right - child.left;
        anime({
          targets: '.square',
          left: child.left - parent.left,
          top: child.top - parent.top,
          height: child.height,
          width: child.width,
          complete: () => {
            console.log('animation ended');
            this.animating = false;
          },
        });
      }
    },
  },
  mounted() {
    this.animate();
  }
};
</script>

<style scoped>
.home {
  position: relative;
  height: 100vh;
  padding: 10px;
  box-sizing: border-box;
}

.square, .rebel-ghost {
  position: absolute;
}

.square, .square-ghost, .rebel-ghost {
  width: 300px;
  height: 45px;
  background-color: greenyellow;
  margin: 0 auto;
}

.square-ghost, .rebel-ghost {
  background-color: grey;
}

.rebel-ghost {
  width: 200px;
  height: 30px;
  bottom: 200px;
  left: 25%;
  z-index: -1;
}
</style>
