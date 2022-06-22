<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps({
  dimMode: Boolean,
  cellSize: Number,
  gap: Number,
  anglex: Number,
  angley: Number,
  perspective: Number,
});

const rotation = ref(0);
const rem = parseInt(getComputedStyle(document.documentElement).fontSize);
</script>

<template>
  <article
    :style="
      props.dimMode == true
        ? `perspective-origin: ${props.anglex}% ${props.angley}%; perspective: ${props.perspective}px;`
        : ''
    "
    class="container"
  >
    <a class="arrow" @click="rotation--">‹</a>
    <div
      :style="
        props.dimMode == true
          ? `transform: rotateY(${-40 * rotation}deg)`
          : 'transform: translateX(-32.8%); width: 280%; overflow-x: hidden;'
      "
      class="carousel"
    >
      <div
        :key="num"
        v-for="num in 9"
        :style="
          `width: ${Number(props.cellSize) / 10}rem;` +
          (props.dimMode == true
            ? `transform: translateX(0) rotateY(${
                (num - 1) * 40
              }deg) translateZ(${
                Math.round(
                  ((Number(props.cellSize) / 20) * rem) / Math.tan(Math.PI / 9)
                ) + Number(props.gap)
              }px)`
            : `margin-left: 7px; transform: translateX(${
                (-rotation + num) * (190 + Number(props.gap))
              }px)`)
        "
        :class="'cell' + (props.dimMode == true ? ' dimCell' : '')"
      >
        {{ num }}
      </div>
    </div>
    <a class="arrow right" @click="rotation++">›</a>
  </article>
</template>

<style scoped>
.container {
  margin: 10% auto;
  width: 14rem;
  height: 9rem;
  position: relative;
}

.carousel {
  width: 100%;
  height: 100%;
  position: absolute;
  transform-style: preserve-3d;
  transition: all 300ms ease-in-out;
}

.cell {
  position: absolute;
  text-align: center;
  font-size: 5rem;
  width: 12rem;
  aspect-ratio: 1.62;
  background: #ee33336d;
  border: 0.16rem solid #ee2233;
  border-radius: 0.6rem;
  transition: all 300ms;
  left: 10px;
  top: 10px;
}

.arrow {
  position: absolute;
  font-size: 3rem;
  cursor: pointer;
  z-index: 1;
  top: 50%;
  left: -120%;
  user-select: none;
  transform: translateY(-50%);
}

.right {
  left: 220%;
}
</style>
