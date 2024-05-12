<template>
  <section class="py-5">
    <div class="container">
      <div class="d-flex align-items-center">
        <h2 class="flex-fill">New Arrivals</h2>
        <a href="#" class="text-decoration-none"
          >View All <span class="visually-hidden">New Arrivals</span></a
        >
      </div>
      <HomeCarousel :products="images" />
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";
import HomeCarousel from "./HomeCarousel.vue";

const images = ref([
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+1",
    altText: "Image 1",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+2",
    altText: "Image 2",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+3",
    altText: "Image 3",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+4",
    altText: "Image 4",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+5",
    altText: "Image 5",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+6",
    altText: "Image 1",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+7",
    altText: "Image 2",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+8",
    altText: "Image 3",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+9",
    altText: "Image 4",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+10",
    altText: "Image 5",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+11",
    altText: "Image 5",
  },
  {
    imageSrc: "https://placehold.co/300x200/000000/F00?text=New+Arrival+12",
    altText: "Image 5",
  },
]);
</script>
