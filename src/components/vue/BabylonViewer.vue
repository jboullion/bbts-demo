<template>
  <div ref="renderCanvas" style="width: 400px; height: 400px" client:load></div>
</template>

<script setup lang="ts">
import { onMounted, ref, nextTick } from "vue";
import {
  Engine,
  Scene,
  ArcRotateCamera,
  HemisphericLight,
  Vector3,
  SceneLoader,
} from "@babylonjs/core";

const renderCanvas = ref<HTMLCanvasElement | null>(null);

onMounted(async () => {
  if (!window) return;
  await nextTick();

  if (renderCanvas.value) {
    const engine = new Engine(renderCanvas.value, true);
    // const scene = new Scene(engine);
    // const camera = new ArcRotateCamera(
    //   "camera",
    //   -Math.PI / 2,
    //   Math.PI / 2.5,
    //   10,
    //   Vector3.Zero(),
    //   scene
    // );
    // camera.attachControl(renderCanvas.value, true);
    // new HemisphericLight("light", new Vector3(1, 1, 0), scene);
    // SceneLoader.ImportMesh(
    //   "",
    //   "/models/",
    //   "lego_voltron.gltf",
    //   scene,
    //   (newMeshes) => {
    //     if (newMeshes.length > 0) {
    //       scene.activeCamera!.target = newMeshes[0].position;
    //     }
    //   }
    // );
    // engine.runRenderLoop(() => {
    //   scene.render();
    // });
    // window.addEventListener("resize", () => {
    //   engine.resize();
    // });
  }
});
</script>
