<template>
  <div ref="mount" style="width: 100%; height: 300px" client:load></div>
</template>

<script setup lang="ts">
import { onMounted, ref, nextTick } from "vue";
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

const mount = ref<HTMLDivElement | null>(null);

onMounted(async () => {
  await nextTick(); // Ensures the component is mounted in the client

  if (mount.value) {
    // Scene
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0x000000);

    // Camera
    const camera = new THREE.PerspectiveCamera(
      75,
      mount.value.clientWidth / mount.value.clientHeight,
      0.1,
      1000
    );
    camera.position.set(0, 1, 2);

    // Renderer
    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(mount.value.clientWidth, mount.value.clientHeight);
    mount.value.appendChild(renderer.domElement);
    renderer.gammaOutput = true; // Ensures colors are managed correctly
    renderer.gammaFactor = 2.2;

    // Orbit Controls
    const controls = new OrbitControls(camera, renderer.domElement);
    controls.update();

    // Lighting
    const ambientLight = new THREE.AmbientLight(0x404040);
    scene.add(ambientLight);

    // Red Spotlights
    const lightPositions = [
      { x: 100, y: 100, z: 100 },
      { x: -100, y: 100, z: 100 },
      { x: 100, y: 100, z: -100 },
      { x: -100, y: 100, z: -100 },
    ];
    lightPositions.forEach((pos) => {
      const spotlight = new THREE.SpotLight(0xdd0000, 10);
      spotlight.position.set(pos.x, pos.y, pos.z);
      spotlight.angle = Math.PI / 6;
      spotlight.penumbra = 0.2;
      spotlight.decay = 2;
      spotlight.distance = 1000;
      scene.add(spotlight);
      // Optional: Visualize the light direction (helpful for debugging)
      const helper = new THREE.SpotLightHelper(spotlight);
      scene.add(helper);
    });

    // GLTF Loader
    const loader = new GLTFLoader();
    loader.load(
      "/models/lego_voltron_moc_r004.glb", // Update the path to your GLB model
      (gltf) => {
        gltf.scene.traverse((child) => {
          if (child.isMesh) {
            console.log(child.material);
            // Upgrade materials to those affected by light
            const mat = new THREE.MeshPhongMaterial({
              color: child.material.color,
              map: child.material.map, // Use existing texture maps
            });
            child.material = mat;
          }
        });
        scene.add(gltf.scene);

        // Calculate the bounding box to correctly frame the model
        const box = new THREE.Box3().setFromObject(gltf.scene);
        const center = box.getCenter(new THREE.Vector3());
        const size = box.getSize(new THREE.Vector3());
        const maxDim = Math.max(size.x, size.y, size.z);
        const fov = camera.fov * (Math.PI / 180);
        const cameraZ = -Math.abs((maxDim / 4) * Math.tan(fov * 2)) * 3; // Adjust camera Z based on the model size

        camera.position.set(center.x, center.y, cameraZ); // Distance camera farther from the center
        camera.lookAt(center); // Look at the center of the model

        // Update the orbit controls to handle the new camera position
        controls.target.copy(center);
        controls.update();

        animate();
      },
      undefined,
      (error) => {
        console.error("An error happened while loading the model:", error);
      }
    );

    // Animation Loop
    const animate = () => {
      requestAnimationFrame(animate);
      controls.update(); // only required if controls.enableDamping or controls.autoRotate are set to true
      renderer.render(scene, camera);
    };

    // Handle Resize
    window.addEventListener("resize", () => {
      camera.aspect = mount.value!.clientWidth / mount.value!.clientHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(mount.value!.clientWidth, mount.value!.clientHeight);
    });
  }
});
</script>
