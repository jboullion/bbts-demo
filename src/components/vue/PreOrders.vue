<template>
  <section class="py-5">
    <div class="container">
      <div class="d-flex align-items-center">
        <h2 class="flex-fill">Pre Orders</h2>
        <a href="#" class="text-decoration-none"
          >View All <span class="visually-hidden">Pre Orders</span></a
        >
      </div>
      <ul class="scroll-list">
        <li v-for="image in images" :key="image.imageSrc" class="m-2">
          <a href="#" class="card">
            <img
              :src="image.imageSrc"
              class="w-100 rounded"
              :alt="image.altText"
              loading="lazy"
            />
          </a>
        </li>
      </ul>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";
import HomeCarousel from "./HomeCarousel.vue";

const images = ref([
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+1",
    altText: "Image 1",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+2",
    altText: "Image 2",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+3",
    altText: "Image 3",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+4",
    altText: "Image 4",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+5",
    altText: "Image 5",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+6",
    altText: "Image 1",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+7",
    altText: "Image 2",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+8",
    altText: "Image 3",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+9",
    altText: "Image 4",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+10",
    altText: "Image 5",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+11",
    altText: "Image 5",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=Pre+Order+12",
    altText: "Image 5",
  },
]);
</script>
<style>
.scroll-list {
  overflow-x: auto;
  white-space: nowrap;
  padding: 0;
  margin: 0 -10px;
  list-style: none;
  display: flex;
}

.scroll-list li {
  flex: 0 0 auto;
}
</style>
