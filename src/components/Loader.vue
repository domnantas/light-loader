<script setup lang="ts">
export interface Props {
  circleCount?: number
  loaderSize?: number
  circleSize?: number
}

const props = withDefaults(defineProps<Props>(), {
  circleCount: 10,
  loaderSize: 200,
  circleSize: 40
})

CSS.registerProperty({
  name: '--hue',
  syntax: '<number>',
  inherits: false,
  initialValue: 0,
});

const getCircleTransform = (circleIndex: number) => {
  const angleBetweenCircles = 360 / props.circleCount;
  return `rotate(${90 + angleBetweenCircles * circleIndex}deg) translate(${props.loaderSize / 2
    }px) rotate(-${90 + angleBetweenCircles * circleIndex}deg)`;
};
</script>

<template>
  <div class="loader">
    <div class="circle-wrapper" v-for="circleIndex in props.circleCount" :key="circleIndex" :style="{
      transform: getCircleTransform(circleIndex),
    }">
      <div class="circle" :style="{
        'animation-delay': `0s, -${circleIndex / props.circleCount
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
  width: calc(v-bind('props.loaderSize') * 1px);
  height: calc(v-bind('props.loaderSize') * 1px);
  border-radius: calc(v-bind('props.loaderSize') * 1px / 2);
  position: relative;
  animation: loader-rotation 2s infinite linear;
  transform-style: preserve-3d;
}

.circle-wrapper {
  width: calc(v-bind('props.circleSize') * 1px);
  height: calc(v-bind('props.circleSize') * 1px);
  position: absolute;
  top: 50%;
  left: 50%;
  margin: calc(v-bind('props.circleSize') * -1px / 2);
  transform-style: preserve-3d;
}

.circle {
  width: 100%;
  height: 100%;
  background-color: hsl(var(--hue, 200), 40%, 70%);
  border-radius: calc(v-bind('props.circleSize') * 1px / 2);
  animation: loader-rotation 2s infinite linear reverse,
    pulse 1s infinite linear, hue-rotate 10s infinite linear;
}
</style>
