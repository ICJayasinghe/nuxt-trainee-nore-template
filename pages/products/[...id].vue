<template>
  <div>
    <ProductDetail v-if="product" :product="product" />
    <div v-else>Loading...</div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { useRoute } from 'vue-router';
import { useFetch } from 'nuxt/app';
import ProductDetail from '~/components/ProductDetail.vue';

interface Product {
  id: string;
  name: string;
  images: string[];
  price: number;
  discount: number;
  installment: number;
  code: string;
  availability: string;
  fabric: string;
  color: string;
  sizes: string[];
  brand: string;
}

const route = useRoute();
const product = ref<Product | null>(null);

// Watch for changes in the route parameters
watch(
  () => route.params.id,
  async (newId) => {
    if (newId) {
      const { data, error } = await useFetch<Product>(`https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data/${newId}`);

      if (data.value) {
        product.value = data.value;
      } else if (error.value) {
        console.error('Error fetching product:', error.value);
      }
    }
  },
  { immediate: true }
);
</script>
