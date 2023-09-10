<template>
  <div id="slider" ref="slides">
    <div class="slide" v-for="(img, k) in images" :key="k">
      <img :src="img" />
    </div>
  </div>
</template>

<script>
import { gsap, Power2 } from "gsap";

export default {
  name: "HelloWorld",
  props: {
    images: Array,
  },
  data() {
    return {
      slides: [],
    };
  },
  created() {
    this.randomPosition();
  },
  mounted() {
    let app = this;
    this.$nextTick(() => {
      app.slides = Array.from(this.$refs.slides.children);
      
      // 随机放置第2到最后一张图片的位置在上下左右左上左下右上右下8个位置
      app.randomPosition();
      function foo() {
        let tl = gsap.timeline({
          onComplete: () => {
            console.log("播放完成，重新设置位置");
            // 重置位置
            app.randomPosition();
            console.log("幻灯片位置", app.slides[0]);
            setTimeout(() => {
              foo();
            }, 1000);
          },
        });
        tl.to(app.slides[0], {
          duration: 3,
          x: 0,
          y: 0,
          ease: Power2.easeOut,
        });
        for (let i = 1; i < app.slides.length; i++) { 
                   tl.to(app.slides[i], { 
            duration: 3,
            x: 0,
            y: 0,
            ease: Power2.easeOut,
          });
        }
      }
      foo();
    });
  },
  methods: {
    randomPosition() {
      let app = this;
      let screenwidth = window.innerWidth;
      let screenheight = window.innerHeight;
      let left = {
        x: -screenwidth,
        y: 0,
      };
      let right = {
        x: screenwidth,
        y: 0,
      };
      let top = {
        x: 0,
        y: -screenheight,
      };
      let bottom = {
        x: 0,
        y: screenheight,
      };
      let topleft = {
        x: -screenwidth,
        y: -screenheight,
      };
      let topright = {
        x: screenwidth,
        y: -screenheight,
      };
      let bottomleft = {
        x: -screenwidth,
        y: screenheight,
      };
      let bottomright = {
        x: screenwidth,
        y: screenheight,
      };
      let position = [
        left,
        right,
        top,
        bottom,
        topleft,
        topright,
        bottomleft,
        bottomright,
      ];

      for (let i = 1; i < app.slides.length; i++) {
        let random = Math.floor(Math.random() * position.length);
        // console.log("位置",random);
        console.log("位置", position[random].x, position[random].y);
        gsap.set(app.slides[i], {
          x: position[random].x,
          y: position[random].y,
        });
        console.log("幻灯片位置", this.slides[i].style.transform);
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#slider {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}
.slide {
  position: absolute;
  width: 100%;
  height: 100%;
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
