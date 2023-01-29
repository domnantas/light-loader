<script setup lang="ts">
import { ref } from 'vue';
const circleCount = ref(10);
const loaderSize = ref(100);
const circleSize = ref(20);

CSS.registerProperty({
  name: '--hue',
  syntax: '<number>',
  inherits: false,
  initialValue: 0,
});

const getCircleTransform = (circleIndex: number) => {
  const angleBetweenCircles = 360 / circleCount.value;
  return `rotate(${90 + angleBetweenCircles * circleIndex}deg) translate(${loaderSize.value / 2
    }px) rotate(-${90 + angleBetweenCircles * circleIndex}deg)`;
};
</script>

<template>
  <div class="loader">
    <div class="circle-wrapper" v-for="circleIndex in circleCount" :key="circleIndex" :style="{
      transform: getCircleTransform(circleIndex),
    }">
      <div class="circle" :style="{
        'animation-delay': `0s, -${circleIndex / circleCount
          }s, -${circleIndex}s`,
      }" />
    </div>
  </div>
</template>

<style scoped>
@keyframes loader-rotation {
  to {
    transform: rotateY(360deg);
  }
}

@keyframes pulse {

  0%,
  30% {
    opacity: 0;
  }

  70% {
    opacity: 1;
  }

  100% {
    opacity: 0;
  }
}

@keyframes hue-rotate {
  100% {
    --hue: 360;
  }
}

.loader {
  width: calc(v-bind(loaderSize) * 1px);
  height: calc(v-bind(loaderSize) * 1px);
  border-radius: calc(v-bind(loaderSize) * 1px / 2);
  position: relative;
  animation: loader-rotation 2s infinite linear;
  transform-style: preserve-3d;
}

.circle-wrapper {
  width: calc(v-bind(circleSize) * 1px);
  height: calc(v-bind(circleSize) * 1px);
  position: absolute;
  top: 50%;
  left: 50%;
  margin: calc(v-bind(circleSize) * -1px / 2);
  transform-style: preserve-3d;
}

.circle {
  width: 100%;
  height: 100%;
  background-color: hsl(var(--hue), 40%, 70%);
  border-radius: calc(v-bind(circleSize) * 1px / 2);
  animation: loader-rotation 2s infinite linear reverse,
    pulse 1s infinite linear, hue-rotate 10s infinite linear;
}
</style>
