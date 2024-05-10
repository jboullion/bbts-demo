<template>
  <div>
    <div id="productCanvas" class="" style="position: relative">
      <i
        v-if="loading"
        class="fas fa-cog fa-spin mb-0 h1 position-absolute top-50 start-50"
      ></i>
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
let animator: number;
let model: GLTF | null;

const animation = {
  prevTick: 0,
  complete: false,
  FPS: 24,
};

const loading = ref(true);

const debouncedResize = throttle(onWindowResize, 32);
/**
 *
 *
 * SETUP
 *
 *
 */

onMounted(async () => {
  await loadModels();

  // updateCoords();

  setupScene();

  window.addEventListener("resize", debouncedResize, { passive: true });
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", debouncedResize);
});

async function loadModels() {
  const loader = new GLTFLoader();

  model = await loader.loadAsync("/models/lego_voltron_moc_r004.glb");
}

function setupScene() {
  setupThreeJS();
  onWindowResize();
  setupModel();
  //setupSceneObjects();

  loading.value = false;

  // if (!animation.prevTick) {
  animate();
  // }
}

// function clearScene(): void {
// 	while (scene.children.length > 0) {
// 		scene.remove(scene.children[0]);
// 	}
// }

function setupThreeJS() {
  scene = new THREE.Scene();

  // Renderer
  renderer = new THREE.WebGLRenderer({
    antialias: true,
    logarithmicDepthBuffer: true,
  }); // { alpha: true }
  canvas = document.getElementById("productCanvas");

  if (!canvas) return;

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
  scene.add(new THREE.AmbientLight(0x404040));

  //   const lensflare = new Lensflare();
  //   lensflare.addElement(new LensflareElement(textureFlare0, 1000, 0));
  //   light.add(lensflare);

  // TODO: Possibly create a point light for the sun
  //const light = new THREE.DirectionalLight(0xffffff, 0.5);
  //scene.add(light);

  // // GUI
  // this.gui = new dat.GUI( { autoPlace: false } );
  // this.canvas.appendChild(this.gui.domElement);
}

function setupModel() {
  if (!model) return;

  model.scene.scale.set(0.04, 0.04, 0.04); // scales to ~2 meters
  model.scene.rotation.set(0, 0, 0);

  scene.add(model.scene);
}

//   three.spaceman = models.spaceman.scene;
//   three.spaceman.scale.set(0.04, 0.04, 0.04); // scales to ~2 meters
//   three.spaceman.rotation.set(0, 0, 0);

//   // Build shape
//   if (formData.value.type.shape === "can") {
//     spacemanPosition += 0.5;
//     three.spaceman.rotation.x = Math.PI / 2;
//     three.spaceman.rotation.y = 1.8;
//     three.spaceman.position.set(1, spacemanPosition, 0); // 6, if solid shape set Z to height of the extrusion

//     if (forces.vectorMesh) {
//       forces.vectorMesh.position.set(
//         three.spaceman.position.x - 1.5,
//         three.spaceman.position.y - 1.5,
//         three.spaceman.position.z
//       );
//     }
//   } else if (formData.value.type.shape === "cylinder") {
//     three.spaceman.rotation.y = -Math.PI / 2;
//     three.spaceman.position.set(
//       0,
//       spacemanPosition,
//       formData.value.shipLength / 2
//     );

//     // If the radius of the structure is too big our spaceman becomes tiny! let's highlight him with a ring.
//     if (formData.value.radius > 30) {
//       const ringGeometry = new THREE.RingGeometry(
//         3,
//         formData.value.radius / 20,
//         32
//       );
//       const ringMaterial = new THREE.MeshBasicMaterial({
//         color: 0xba3700,
//         side: THREE.DoubleSide,
//       });
//       const ringMesh = new THREE.Mesh(ringGeometry, ringMaterial);

//       ringMesh.position.set(
//         0,
//         spacemanPosition + 2,
//         formData.value.shipLength / 2
//       );

//       three.group.add(ringMesh);
//     }
//   } else if (formData.value.type.shape === "bola") {
//     three.spaceman.rotation.y = -Math.PI / 2;
//     three.spaceman.position.set(0, spacemanPosition, 0);

//     // If the radius of the structure is too big our spaceman becomes tiny! let's highlight him with a ring.
//     if (stationWidth.value > 30) {
//       const ringGeometry = new THREE.RingGeometry(
//         3,
//         Math.max(formData.value.shipWidth - 2, 4),
//         32
//       );
//       const ringMaterial = new THREE.MeshBasicMaterial({
//         color: 0xba3700,
//         side: THREE.DoubleSide,
//       });
//       const ringMesh = new THREE.Mesh(ringGeometry, ringMaterial);

//       ringMesh.position.set(0, spacemanPosition + 2, 0);

//       three.group.add(ringMesh);
//     }
//   } else if (formData.value.type.shape === "funnel") {
//     spacemanPosition += 1;

//     three.spaceman.rotation.z = forces.vector; //Math.PI / 2;
//     //three.spaceman.rotation.y = 1.8;
//     three.spaceman.position.set(spacemanPosition, 0, 0); // 6, if solid shape set Z to height of the extrusion

//     //three.group.rotation.x = -Math.PI / 2.5;

//     // If the radius of the structure is too big our spaceman becomes tiny! let's highlight him with a ring.
//     if (formData.value.radius > 30) {
//       const ringGeometry = new THREE.RingGeometry(
//         formData.value.shipLength,
//         formData.value.shipLength * 1.3,
//         32
//       );
//       const ringMaterial = new THREE.MeshBasicMaterial({
//         color: 0xba3700,
//         side: THREE.DoubleSide,
//       });
//       const ringMesh = new THREE.Mesh(ringGeometry, ringMaterial);

//       ringMesh.rotation.z = forces.vector;

//       ringMesh.position.set(spacemanPosition + 2, 0, 0);

//       three.group.add(ringMesh);
//     }

//     if (forces.vectorMesh) {
//       forces.vectorMesh.position.set(three.spaceman.position.x, -2, 0);
//     }
//   }
//   // // Gui folder
//   // const spacemanFolder = three.gui.addFolder('Spaceman');
//   // spacemanFolder.add(three.spaceman.position, 'x', 0, stationWidth.value).name("px");
//   // spacemanFolder.add(three.spaceman.position, 'y', -stationWidth.value * 2, stationWidth.value * 2).name("py");
//   // spacemanFolder.add(three.spaceman.position, 'z', 0, stationWidth.value).name("pz");

//   // spacemanFolder.add(three.spaceman.rotation, 'x', -Math.PI / 2, 0).name("rx");
//   // spacemanFolder.add(three.spaceman.rotation, 'y', -Math.PI / 2, 0).name("ry");
//   // spacemanFolder.add(three.spaceman.rotation, 'z', -Math.PI / 2, 0).name("rz");
//   // spacemanFolder.open();

//   three.group.add(three.spaceman);

// }

function onWindowResize() {
  if (!camera) return;
  if (!canvas) return;
  if (!renderer) return;

  const width = canvas.getBoundingClientRect().width;
  const height = width * 0.67;

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
  const standardDistance = 1000;
  const cameraPositionDistance = standardDistance * 2.5;
  const cameraZoomDistance = standardDistance * 10;

  let rendererSize = new THREE.Vector2();
  renderer.getSize(rendererSize);
  camera = new THREE.PerspectiveCamera(
    45,
    rendererSize.width / rendererSize.height,
    0.1,
    cameraZoomDistance * 2
  );

  camera.position.z = cameraPositionDistance;
  camera.position.y -= cameraPositionDistance;
  camera.lookAt(0, 0, 0);

  // controls.enableZoom = false;

  // Controls
  controls = new OrbitControls(camera, renderer.domElement);
  // controls.enableDamping = true;
  // thr.controls.dampingFactor = 0.05;
  controls.maxDistance = cameraZoomDistance;
  controls.minDistance = 100;
}

function animate() {
  if (!renderer) return;
  if (!controls) return;
  if (loading.value) return;

  animator = requestAnimationFrame(animate);

  controls.update();

  //this.camera.position.clamp(this.minMovement, this.maxMovement);
  renderer.render(scene, camera);
  //if (labelRenderer) labelRenderer.render(scene, camera);

  // clamp to fixed framerate

  const now = Math.round((animation.FPS * window.performance.now()) / 1000);

  if (now == animation.prevTick) return;

  animation.prevTick = now;
}
</script>
<style></style>
