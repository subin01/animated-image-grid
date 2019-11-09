<template>
  <div class="image-grid">
    <div class="wrap">
      <article class="boxes">
        <div
          class="box"
          :key="index"
          v-for="(item, index) in (grid[0] * grid[1])"
          :class="{ 'active': activeIndex === index }"
          :style="getStyle(index)"
          @click="handleClick(index)"
        >
          <img :src="`./images/thumbs/img${index}.jpg`" width="100%" height="100%" />
        </div>
      </article>

      <section class="preview" id="preview">
        <img
          :src="`./images/img${activeIndex}.jpg`"
          v-if="isOpen"
          width="100%"
          height="100%"
          @click="handleClose()"
        />
      </section>
    </div>
  </div>
</template>

<script>
const BOX_SIZE = 200;
const BOX_GUTTER = 20;
import { TimelineLite, Power3, Linear, Elastic } from 'gsap';
export default {
  name: 'List2',
  props: {
    msg: String,
  },
  data() {
    return {
      grid: [5, 3],
      activeIndex: null,
    };
  },
  computed: {
    isOpen() {
      return this.activeIndex !== null;
    },
  },
  methods: {
    getStyle(index) {
      const { left, top } = this.getPosition(index);
      return { left: `${left}px`, top: `${top}px` };
    },

    getPosition(index) {
      const columns = 5;
      let leftMultiplier = index % columns;
      let topMultiplier = Math.floor(index / columns);

      return {
        left: leftMultiplier * BOX_SIZE + leftMultiplier * BOX_GUTTER,
        top: topMultiplier * BOX_SIZE + topMultiplier * BOX_GUTTER,
      };
    },

    setActive(index) {
      if (index === this.activeIndex) this.activeIndex = null;
      else this.activeIndex = index;
    },

    handleClick(index) {
      this.setActive(index);
      this.openPreview();
    },

    openPreview() {
      const { left, top } = this.getPosition(this.activeIndex);
      const timeline = new TimelineLite();
      timeline
        .to('#preview', 0, {
          x: left,
          y: top,
          width: BOX_SIZE,
          height: BOX_SIZE,
        })
        .staggerTo('.box', 0.6, {
          ease: Linear.easeNone,
          scaleY: 3,
          scaleX: 0.5,
          y: 1600,
          opacity: 0.2,
          stagger: {
            ease: Elastic.easeOut,
            amount: 0.5,
            axis: 'x',
            from: this.activeIndex,
          },
        })
        .to(
          '#preview',
          0.6,
          {
            ease: Power3.easeIn,
            x: 0,
            y: 0,
            width: '100%',
            height: '50rem',
          },
          '-=1',
        );
    },

    handleClose() {
      const { preview } = this.$refs;
      const { left, top } = this.getPosition(this.activeIndex);

      const timeline = new TimelineLite();

      timeline
        .staggerTo('.box', 0.8, {
          ease: Elastic.easeOut,
          scaleX: 1,
          scaleY: 1,
          y: 0,
          opacity: 1,
          stagger: {
            grid: this.grid,
            amount: 0.5,
            axis: 'x',
            from: 'start',
            ease: Linear.easeNone,
          },
        })
        .to(
          '#preview',
          0.5,
          {
            x: left,
            y: top,
            width: BOX_SIZE,
            height: BOX_SIZE,
            onComplete: () => this.reset(timeline),
          },
          '-=1',
        );
    },
    reset(timeline) {
      this.activeIndex = null;
      timeline.to('#preview', 0.1, {
        height: 0,
        width: 0,
      });
    },
  },
  mounted() {
    this.timeline = new TimelineLite();
  },
};
</script>

<style lang="scss">
$size: 200px;

.image-grid {
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  display: flex;
  align-content: center;
  justify-content: center;
}

.wrap {
  width: 70rem;
  position: relative;
}

.boxes {
  width: 100%;
  float: left;
}

.box {
  width: $size;
  height: $size;
  position: absolute;
  display: flex;
  align-content: center;
  justify-content: center;
}

.box.active {
  opacity: 0 !important;
}

.preview {
  position: absolute;
  overflow: hidden;
}
</style>
