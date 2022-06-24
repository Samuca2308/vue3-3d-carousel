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

const rotation = ref(104);
const rem = parseInt(getComputedStyle(document.documentElement).fontSize);
const carousel: Ref<HtmlElement | null> = ref(null);

function rotate(clockwise: Boolean) {
  clockwise ? rotation.value-- : rotation.value++;
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
      ref="carousel"
      :style="
        props.dimMode == true
          ? `left: ${(14 - cellRem) / 2}rem; width: ${
              cellRem + 2
            }rem; transform: rotateY(${-45 * rotation}deg)`
          : 'transform: translateX(-25%); width: 200%;'
      "
      :class="`carousel ${props.dimMode == true ? '' : 'td'}`"
    >
      <div
        :key="num"
        v-for="num in 8"
        :style="
          `width: ${cellRem}rem;` +
          (props.dimMode == true
            ? `transform: translateX(0) rotateY(${
                (num - 1) * (360 / 8)
              }deg) translateZ(${
                Math.round(((cellRem / 2) * rem) / Math.tan(Math.PI / 8)) + gap
              }px)`
            : `transform: translateX(
              
              
              ${
                -5 * rem + // -5rem
                (num == 8 && rotation % 8 == 0
                  ? 0
                  : -(rotation % 8) + // - index
                    (num > (rotation % 8) - 2 && num < (rotation % 8) + 6 // cut in half
                      ? 0
                      : 8) +
                    num) *
                  (cellRem * rem + gap) + //
                (12 - cellRem) * 1.5 * rem -
                gap
              }px);
              
              opacity: ${
                rotation % 8 == 7 && (num == 7 || num == 8 || num == 1)
                  ? '1'
                  : rotation % 8 == 0 && (num == 8 || num == 1 || num == 2)
                  ? '1'
                  : num > (rotation % 8) + 2 || num < rotation % 8
                  ? '0'
                  : '1'
              };`)
        "
        :class="'cell' + (props.dimMode == true ? ' dimCell' : '')"
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
  transition: transform 300ms ease-in-out;
}

.td::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
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
  transition: transform 300ms ease-in-out;
  border-radius: 0.6rem;
  color: #ce7e00;
  left: 10px;
  top: 10px;
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
