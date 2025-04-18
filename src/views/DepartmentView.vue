<template>
  <FrameWrapper>
    <DeptDetailSwitch></DeptDetailSwitch>
    <div class="vfx-contain">
      <canvas class="vfx" ref="vfx" :width="width" :height="height"></canvas>
    </div>
    <div class="background"></div>
  </FrameWrapper>
</template>

<script setup lang="ts">
import DeptDetailSwitch from '@/components/DeptDetailSwitch.vue';
import FrameWrapper from '@/components/FrameWrapper.vue';
import { onMounted, onUnmounted, useTemplateRef } from 'vue';
import {
  startAnimation, stopAnimation, updateWindowSize
} from '@/package/points';
import { ref } from 'vue';

const canvas = useTemplateRef("vfx")
const height = ref(0)
const width = ref(0)
let resizing: boolean = false

function resizeHandler() {
  if (!resizing) {
    resizing = true
    setTimeout(() => {
      resizing = false;
      updateWindowSize();
    }, 250);
  }
}

onMounted(() => {
  height.value = 1080;
  width.value = 1920;
  window.addEventListener("resize", resizeHandler)
  startAnimation(canvas.value as HTMLCanvasElement)
})

onUnmounted(() => {
  window.removeEventListener('resize', resizeHandler)
  stopAnimation()
})

</script>

<style scoped>

div.vfx-contain {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  container-type: size;
  pointer-events: none;
  height: 100%;
  width: 100%;
  z-index: -1;
}

canvas.vfx {
  position: relative;
  pointer-events: none;
  width: 100cqmax;
  height: 56.25cqmax;
}

div.background {
  position: absolute;
  width: 100%;
  height: 100%;
  background-image:
    linear-gradient(rgba(255,255,255,.5) 2px, transparent 2px),
    linear-gradient(90deg, rgba(255,255,255,.5) 2px, transparent 2px),
    linear-gradient(rgba(255,255,255,.3) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,.3) 1px, transparent 1px);
  background-size: 100px 100px, 100px 100px, 20px 20px, 20px 20px;
  background-position:-2px -2px, -2px -2px, -1px -1px, -1px -1px;
  mask-image: radial-gradient(rgba(0, 0, 0, 0.2), transparent 80%);
  mask-mode: alpha;

  transition:
    scale 1.5s ease,
    opacity 1.5s ease;
  
  z-index: -3;
}

.forward-enter-from div.background {
  opacity: 0;
  scale: 0.5 0.5;
}
.rewind-enter-from div.background {
  opacity: 0;
  scale: 0.5 0.5;
}

.forward-enter-from :deep(div.button) {
  opacity: 0;
}
.rewind-enter-from :deep(div.button) {
  opacity: 0;
}

.forward-enter-from :deep(div.container) {
  opacity: 0;
}
.rewind-enter-from :deep(div.container) {
  opacity: 0;
}

</style>