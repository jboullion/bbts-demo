<template>
    <section class="py-5">
        <div class="container">
            <div class="d-flex align-items-center">
                <h2 class="flex-fill">New Arrivals</h2>
                <a href="#" class="text-decoration-none">View All</a>
            </div>
            <Carousel :value="images" :numVisible="4" :numScroll="4" :showIndicators="false" circular :responsiveOptions="responsiveOptions">
                <template #item="slotProps">
                    <a href="#" class="card m-2">
                        <img :src="slotProps.data.imageSrc" class="w-100" :alt="slotProps.data.altText">
                    </a>
                </template>
            </Carousel>
        </div>
    </section>
</template>
  
<script setup>
  import Carousel from 'primevue/carousel';
  import { ref } from 'vue';
  
  const responsiveOptions = ref([
    {
        breakpoint: '1400px',
        numVisible: 4,
        numScroll: 4
    },
    {
        breakpoint: '1199px',
        numVisible: 3,
        numScroll: 3
    },
    {
        breakpoint: '767px',
        numVisible: 2,
        numScroll: 2
    },
    {
        breakpoint: '575px',
        numVisible: 1,
        numScroll: 1
    }
]);


  const images = ref([
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+1', altText: 'Image 1' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+2', altText: 'Image 2' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+3', altText: 'Image 3' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+4', altText: 'Image 4' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+5', altText: 'Image 5' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+6', altText: 'Image 1' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+7', altText: 'Image 2' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+8', altText: 'Image 3' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+9', altText: 'Image 4' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+10', altText: 'Image 5' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+11', altText: 'Image 5' },
    { imageSrc: 'https://placehold.co/300x200/000000/F00?text=New+Arrival+12', altText: 'Image 5' },
  ]);
</script>
<style>
    .p-carousel {
        margin: 0 -36px;
    }

    .p-carousel-indicators {
        margin: 0;
    }

    .p-carousel-next.p-link {
        left: -20px;
    }

    .p-carousel-prev.p-link {
        right: -20px;
        z-index: 2;
    }

    .p-carousel-next.p-link,
    .p-carousel-prev.p-link,
    .p-carousel-next.p-link:hover,
    .p-carousel-prev.p-link:hover {
        color: black;
        border-radius: 50%;
        background-color: white;
        border-color: red;
        border: 1px solid red;
    }

    @media screen and (max-width: 575px){
        .p-carousel {
            margin: 0;
        }

        .p-carousel-next.p-link,
        .p-carousel-prev.p-link {
            display: none;
        }
    }
</style>