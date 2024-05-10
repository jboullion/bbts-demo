<template>
  <canvas ref="bjsCanvas" width="400" height="300" />
</template>

<script setup lang="ts">
import { ref, onMounted } from "@vue/runtime-core";
import { createScene } from "../../scenes/MyFirstScene";

const bjsCanvas = ref<HTMLCanvasElement | null>(null);

onMounted(async () => {
  if (bjsCanvas.value) {
    createScene(bjsCanvas.value);
  }
});
</script>
