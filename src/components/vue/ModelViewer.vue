<template>
  <div id="productCanvasWrapper" class="d-flex flex-column h-100">
    <div id="productCanvas" class="flex-fill" style="position: relative">
      <div
        v-if="loading"
        class="spinner-border text-danger mb-0 position-absolute top-50 start-50"
        role="status"
      >
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import Stats from "three/examples/jsm/libs/stats.module.js";
import { GUI } from "three/examples/jsm/libs/lil-gui.module.min.js";
import { onBeforeUnmount, onMounted, ref } from "vue";
import { throttle } from "@/utils/utils";
import { GLTF, GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";

let controls: OrbitControls;
let renderer: THREE.WebGLRenderer;
let stats: Stats;
let meshes: THREE.Mesh[];
let camera: THREE.PerspectiveCamera;
let scene: THREE.Scene;
let canvas: HTMLElement | null;
let canvasWrapper: HTMLElement | null;
let animator: number;
let model: GLTF | null;

const animation = {
  prevTick: 0,
  complete: false,
  FPS: 24,
};

const loading = ref(true);

const debouncedResize = throttle(onWindowResize, 32);

onMounted(async () => {
  await loadModels();

  setupScene();

  window.addEventListener("resize", debouncedResize, { passive: true });
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", debouncedResize);
});

async function loadModels() {
  const loader = new GLTFLoader();

  model = await loader.loadAsync(
    "/models/kyouko_kirigiri_figure_from_danganronpa.glb"
  );
}

function setupScene() {
  setupThreeJS();
  onWindowResize();
  setupModel();

  loading.value = false;

  animate();
}

function setupThreeJS() {
  scene = new THREE.Scene();
  scene.background = new THREE.Color(0xb30303);

  // Renderer
  renderer = new THREE.WebGLRenderer({
    antialias: true,
    logarithmicDepthBuffer: true,
  }); // { alpha: true }
  canvas = document.getElementById("productCanvas");
  canvasWrapper = document.getElementById("productCanvasWrapper");

  if (!canvas || !canvasWrapper) return;

  canvas.appendChild(renderer.domElement);

  onWindowResize();
  setupCamera();

  // labelRenderer = new CSS2DRenderer();

  // labelRenderer.setSize(width, height);
  // labelRenderer.domElement.style.position = 'absolute';
  // labelRenderer.domElement.style.top = '0px';
  // labelRenderer.domElement.style.pointerEvents = 'none';
  // canvas.appendChild(labelRenderer.domElement);

  // Lights
  scene.add(new THREE.AmbientLight(0xffffff, 1));

  //   const lensflare = new Lensflare();
  //   lensflare.addElement(new LensflareElement(textureFlare0, 1000, 0));
  //   light.add(lensflare);

  // TODO: Possibly create a point light for the sun
  const light = new THREE.DirectionalLight(0xffffff, 1);
  scene.add(light);

  // // GUI
  // this.gui = new dat.GUI( { autoPlace: false } );
  // this.canvas.appendChild(this.gui.domElement);
}

function setupModel() {
  if (!model) return;

  model.scene.scale.set(3, 3, 3);
  model.scene.rotation.set(0, 2.5, 0);
  model.scene.position.set(0, -20, 0);

  scene.add(model.scene);
}

function onWindowResize() {
  if (!camera) return;
  if (!canvas || !canvasWrapper) return;
  if (!renderer) return;
  canvas.style.height = "0px";

  const width = canvas.getBoundingClientRect().width;
  let height = canvasWrapper.getBoundingClientRect().height;

  if (height < 300) {
    height = 300;
    canvas.style.height = "300px";
  }

  // Update aspect ratio
  camera.aspect = width / height;

  // Update the camera's projection matrix
  camera.updateProjectionMatrix();

  renderer.setSize(width, height);
  canvas.style.paddingTop = `0px`;
}

function setupCamera() {
  if (!renderer) return;

  // Camera
  const standardDistance = 75;
  const cameraPositionDistance = standardDistance;
  const cameraZoomDistance = standardDistance * 6;

  let rendererSize = new THREE.Vector2();
  renderer.getSize(rendererSize);
  camera = new THREE.PerspectiveCamera(
    45,
    rendererSize.width / rendererSize.height,
    0.1,
    cameraZoomDistance * 2
  );

  camera.position.z = cameraPositionDistance;

  camera.lookAt(0, 0, 0);

  // controls.enableZoom = false;

  // Controls
  controls = new OrbitControls(camera, renderer.domElement);
  // controls.enableDamping = true;
  // thr.controls.dampingFactor = 0.05;
  controls.maxDistance = cameraZoomDistance;
  controls.minDistance = standardDistance / 4;
}

function animate() {
  if (!renderer) return;
  if (!controls) return;
  if (loading.value) return;

  animator = requestAnimationFrame(animate);

  controls.update();

  renderer.render(scene, camera);
  //if (labelRenderer) labelRenderer.render(scene, camera);

  // clamp to fixed framerate
  const now = Math.round((animation.FPS * window.performance.now()) / 1000);

  if (now == animation.prevTick) return;

  animation.prevTick = now;
}
</script>
<style></style>
