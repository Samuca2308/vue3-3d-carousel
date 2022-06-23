<script setup lang="ts">
import { ref, toRef, computed } from 'vue';

const props = defineProps({
  dimMode: Boolean,
  cellSize: Number,
  gap: Number,
  anglex: Number,
  angley: Number,
  perspective: Number,
});

const cellSize = toRef(props, 'cellSize');
const gap = toRef(props, 'gap');
const cellRem = computed(() => {
  return cellSize.value / 10;
});

const rotation = ref(0);
const rem = parseInt(getComputedStyle(document.documentElement).fontSize);

function rotate(clockwise: Boolean) {
  clockwise ? (rotation.value--, func()) : (rotation.value++, func());
}

function func() {
  if (rotation.value < 0) {
    var cols = document.getElementsByClassName('transition');
    for (let i = 0; i < cols.length; i++) {
      cols[i].classList.remove('transition');
    }
    rotation.value = 7;
  }
}
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
          ? `left: ${
              (14 - cellRem) / 2
            }rem; width: ${cellRem}rem; transform: rotateY(${
              -45 * rotation
            }deg)`
          : 'transform: translateX(-25%); width: 200%; overflow-x: hidden;'
      "
      :class="`carousel transition ${props.dimMode == true ? '' : 'td'}`"
    >
      <div
        :key="num"
        v-for="num in 8"
        :style="
          `width: ${cellRem}rem;
          filter: hue-rotate(${(360 / 8) * num}deg);` +
          (props.dimMode == true
            ? `transform: translateX(0) rotateY(${
                (num - 1) * (360 / 8)
              }deg) translateZ(${
                Math.round(((cellRem / 2) * rem) / Math.tan(Math.PI / 8)) + gap
              }px)`
            : `transform: translateX(${
                -5 * rem +
                (-rotation + num) * (cellRem * rem + gap) +
                (12 - cellRem) * 1.5 * rem -
                gap
              }px)`)
        "
        :class="'cell transition' + (props.dimMode == true ? ' dimCell' : '')"
      >
        {{ num }}
      </div>
    </div>
    <a :style="`left: -4rem;`" class="arrow" @click="rotate(true)">‹</a>
    <a :style="`left: 17rem;`" class="arrow right" @click="rotate(false)">›</a>
  </article>
</template>

<style scoped>
.container {
  margin: 10% auto 8rem auto;
  width: 14rem;
  height: 9rem;
  position: relative;
}

.carousel {
  height: 20rem;
  position: absolute;
  transform-style: preserve-3d;
}

.td::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  box-shadow: inset 0 0 1.2rem 4rem white;
  transform: translateY(-6rem);
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
  left: 10px;
  top: 10px;
}

.transition {
  transition: transform 300ms ease-in-out;
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
