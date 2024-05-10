<template>
  <Carousel
    :value="products"
    :numVisible="numVisible"
    :numScroll="numScroll"
    :showIndicators="showIndicators"
    :circular="circular"
    :responsiveOptions="responsiveOptions"
  >
    <template #item="slotProps">
      <a href="#" class="card m-2">
        <img
          :src="slotProps.data.imageSrc"
          class="w-100 rounded"
          :alt="slotProps.data.altText"
          loading="lazy"
        />
      </a>
    </template>
  </Carousel>
</template>

<script setup lang="ts">
import Carousel from "primevue/carousel";
import { PropType, ref } from "vue";

type Product = {
  imageSrc: string;
  altText: string;
};

interface Props {
  products: Product[];
  numVisible?: number;
  numScroll?: number;
  showIndicators?: boolean;
  circular?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  numVisible: 4,
  numScroll: 4,
  showIndicators: false,
  circular: true,
});

const responsiveOptions = ref([
  {
    breakpoint: "1400px",
    numVisible: 4,
    numScroll: 4,
  },
  {
    breakpoint: "1199px",
    numVisible: 3,
    numScroll: 3,
  },
  {
    breakpoint: "767px",
    numVisible: 2,
    numScroll: 2,
  },
  {
    breakpoint: "575px",
    numVisible: 1,
    numScroll: 1,
  },
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

@media screen and (max-width: 575px) {
  .p-carousel {
    margin: 0 -15px;
  }

  .p-carousel-next.p-link,
  .p-carousel-prev.p-link {
    position: absolute;
  }

  .p-carousel-next.p-link {
    left: 94% !important;
  }

  .p-carousel-prev.p-link {
    right: 94% !important;
  }
}
</style>
