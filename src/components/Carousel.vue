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
    <div
      :style="
        props.dimMode == true
          ? `left: ${(14 - Number(props.cellSize) / 10) / 2}rem; width: ${
              Number(props.cellSize) / 10
            }rem; transform: rotateY(${-45 * rotation}deg)`
          : 'transform: translateX(-25%); width: 200%; overflow-x: hidden;'
      "
      class="carousel"
    >
      <div
        :key="num"
        v-for="num in 8"
        :style="
          `width: ${Number(props.cellSize) / 10}rem;
          filter: hue-rotate(${(360 / 8) * num}deg);` +
          (props.dimMode == true
            ? `transform: translateX(0) rotateY(${
                (num - 1) * (360 / 8)
              }deg) translateZ(${
                Math.round(
                  ((Number(props.cellSize) / 20) * rem) / Math.tan(Math.PI / 8)
                ) + Number(props.gap)
              }px)`
            : `transform: translateX(${
                -5 * rem +
                (-rotation + num) *
                  ((Number(props.cellSize) / 10) * rem + Number(props.gap)) +
                (12 - Number(props.cellSize) / 10) * 1.5 * rem -
                Number(props.gap)
              }px)`)
        "
        :class="'cell' + (props.dimMode == true ? ' dimCell' : '')"
      >
        {{ num }}
      </div>
    </div>
    <a :style="`left: -4rem;`" class="arrow" @click="rotation--">‹</a>
    <a :style="`left: 17rem;`" class="arrow right" @click="rotation++">›</a>
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
  height: 20rem;
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
  background: #ce7eff;
  border: 0.16rem solid #ce7e88;
  border-radius: 0.6rem;
  color: #ce7e00;
  transition: all 300ms;
  left: 10px;
  top: 10px;
}

.cell:hover {
}

.arrow {
  position: absolute;
  font-size: 3rem;
  cursor: pointer;
  top: 50%;
  user-select: none;
  transform: translateY(-50%);
}
</style>
