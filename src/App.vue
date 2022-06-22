<template>
  <Carousel
    :cellSize="cellSize"
    :gap="gap"
    :dimMode="dimMode"
    :perspective="perspective"
    :anglex="angleX"
    :angley="angleY"
  />
  <aside>
    <section aria-labelledby="genConfig" class="options-container">
      <h3 id="genConfig">Settings</h3>
      <a
        value="dimMode"
        @click="dimMode = !dimMode"
        :class="`switch ${dimMode ? 'switch-selected' : ''}`"
        >3D Carousel</a
      >
      <label for="gap">Gap</label>
      <input
        id="gap"
        type="range"
        min="0"
        max="32"
        v-model="gap"
        class="slider"
      />
      <label for="cell-size">Cell size</label>
      <input
        id="cell-size"
        type="range"
        min="30"
        max="200"
        v-model="cellSize"
        class="slider"
      />
    </section>
    <section
      v-show="dimMode"
      aria-labelledby="dimConfig"
      class="options-container"
    >
      <h3 id="dimConfig">3D mode Settings</h3>
      <div>
        <label for="perspective">Perspective</label>
        <input
          id="perspective"
          type="range"
          min="900"
          max="1600"
          v-model="perspective"
          class="slider"
        />
      </div>
      <div class="track">
        <div
          @mousedown="onMouseDown"
          :style="`left: ${1.8 - (angleX - 50) / 100}rem; top: ${
            1.8 - (angleY - 50) / 100
          }rem`"
          class="complex-slider"
        ></div>
      </div>
    </section>
  </aside>
</template>

<script setup lang="ts">
import { onMounted, Ref, ref } from 'vue';
import Carousel from './components/Carousel.vue';

const dimMode = ref(false);
const cellSize = ref(120);
const gap = ref(16);
const angleX = ref(50);
const angleY = ref(50);
const perspective = ref(1250);

var pos1 = 0,
  pos2 = 0,
  pos3 = 0,
  pos4 = 0;
function onMouseDown(e: MouseEvent) {
  pos3 = e.clientX;
  pos4 = e.clientY;
  document.onmouseup = closeDragElement;
  document.onmousemove = elementDrag;
}

function elementDrag(e: any) {
  pos1 = pos3 - e.clientX;
  pos2 = pos4 - e.clientY;
  pos3 = e.clientX;
  pos4 = e.clientY;

  let ax = angleX.value + (pos1 * 100) / 16;
  ax <= 150 && ax >= -100 ? (angleX.value = ax) : null;
  let ay = angleY.value + (pos2 * 100) / 16;
  ay <= 150 && ay >= -100 ? (angleY.value = ay) : null;
  console.log(angleX.value, angleY.value);
}

function closeDragElement() {
  document.onmouseup = null;
  document.onmousemove = null;
}
</script>

<style>
body {
  margin: 0;
}

section {
  position: relative;
}

aside {
  display: flex;
  justify-content: center;
}

label {
  margin-right: 0.4rem;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.options-container > h3 {
  position: absolute;
  top: -4.6rem;
}

.options-container {
  display: flex;
  width: max-content;
  justify-content: space-around;
}

.switch {
  cursor: pointer;
  display: block;
  position: relative;
  min-width: 6rem;
  margin-inline: 0.8rem;
  margin-bottom: 1.6rem;
}

.switch::before {
  content: '';
  position: absolute;
  top: 1.9rem;
  width: 2.6rem;
  height: 0.2rem;
  background: #2c3e50;
  opacity: 0.7;
  transition: opacity 300ms;
  border-radius: 0.2rem;
}

.switch:hover::before {
  opacity: 1;
}

.switch::after {
  content: '';
  position: absolute;
  left: 0;
  top: 1.4rem;
  margin-left: 0.1rem;
  width: 1.3rem;
  height: 1.3rem;
  border-radius: 50%;
  background-color: darkgray;
  transition: all 300ms;
  box-shadow: 0.1rem 0.1rem 0.2rem 0.1rem rgba(44, 62, 80, 0.26);
}

.switch-selected::after {
  margin-left: 1.2rem;
  background-color: turquoise;
}

.track {
  position: relative;
  height: 3.6rem;
  aspect-ratio: 1;
  background-color: lightgray;
  outline: 0.16rem solid rgba(44, 62, 80, 0.36);
  transition: outline 300ms, background-color 300ms;
  border-radius: 50%;
}

.track:hover {
  background-color: gainsboro;
  outline: 0.16rem solid rgba(44, 62, 80, 0.76);
}

.track:hover .complex-slider {
  opacity: 1;
}

.complex-slider {
  cursor: pointer;
  position: absolute;
  height: 1.3rem;
  aspect-ratio: 1;
  background-color: darkgray;
  box-shadow: 0.1rem 0.1rem 0.2rem 0.1rem rgba(44, 62, 80, 0.16);
  border-radius: 50%;
  opacity: 0.7;
  transition: opacity 300ms;
  transform: translate(-50%, -50%);
}

.slider {
  -webkit-appearance: none;
  margin-right: 1.6rem;
  width: 12rem;
  height: 0.2rem;
  background: #2c3e50;
  outline: none;
  opacity: 0.7;
  border-radius: 0.2rem;
  transition: opacity 300ms;
}

.slider:hover {
  opacity: 1;
}

.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 1.3rem;
  height: 1.3rem;
  border-radius: 50%;
  background-color: darkgray;
  box-shadow: 0.1rem 0.1rem 0.2rem 0.1rem rgba(44, 62, 80, 0.26);
  cursor: pointer;
}

.slider::-moz-range-thumb {
  width: 1.3rem;
  height: 1.3rem;
  border-radius: 50%;
  background-color: darkgray;
  box-shadow: 0.1rem 0.1rem 0.2rem 0.1rem rgba(44, 62, 80, 0.26);
  cursor: pointer;
}
</style>
