<template>
  <Carousel
    :cellSize="Number(cellSize)"
    :gap="Number(gap)"
    :dimMode="tridimensionalMode"
    :perspective="Number(perspective)"
    :anglex="angleX"
    :angley="angleY"
  />
  <aside>
    <section aria-labelledby="general-settings" class="settings-container">
      <h3 id="general-settings">Settings</h3>
      <a
        value="tridimensionalMode"
        @click="tridimensionalMode = !tridimensionalMode"
        :class="`switch ${tridimensionalMode ? 'switch-selected' : ''}`"
        >3D Carousel</a
      >
      <div>
        <label for="gap">Gap</label>
        <input
          id="gap"
          type="range"
          min="0"
          max="32"
          v-model="gap"
          class="slider"
        />
        <br />
        <label for="cell-size">Cell size</label>
        <input
          id="cell-size"
          type="range"
          min="90"
          max="160"
          v-model="cellSize"
          class="slider"
        />
      </div>
    </section>
    <section
      v-show="tridimensionalMode === true"
      aria-labelledby="tridimensional-settings"
      class="settings-container"
    >
      <h3 id="tridimensional-settings">3D mode Settings</h3>
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
        <span>Angle</span>
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

const tridimensionalMode: Ref<Boolean> = ref(false);
const cellSize: Ref<Number> = ref(120);
const gap: Ref<Number> = ref(16);
const angleX: Ref<Number> = ref(50);
const angleY: Ref<Number> = ref(50);
const perspective: Ref<Number> = ref(1250);

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
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

label {
  margin-right: 0.4rem;
  user-select: none;
}

input {
  margin-block: 0.8rem;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.settings-container > h3 {
  position: absolute;
  top: -4.6rem;
}

.settings-container {
  margin-bottom: 6rem;
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

.track > span {
  position: absolute;
  top: -1.6rem;
  user-select: none;
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
