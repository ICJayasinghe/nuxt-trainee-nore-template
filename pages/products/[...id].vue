<template>
  <div class="bg-white text-black">
    <ProductDetail v-if="product" :product="product" />
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

// Function to fetch product data
const fetchProduct = async (productId: string) => {
  try {
    const { data, error } = await useFetch<Product>(`https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data/`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify({
        currency_code: 'LKR', 
        id: productId,

      }),
    });

    if (error.value) {
      throw new Error(error.value.message);
    }

    if (data.value) {
      product.value = data.value;
    } else {
      product.value = null;
    }
  } catch (error) {
    console.error('Error fetching product:', error);
    product.value = null;
  }
};

// Watch for changes in the route parameters
watch(
  () => route.params.id,
  async (newId) => {
    if (newId) {
      await fetchProduct(newId as string);
    }
  },
  { immediate: true }
);
</script>

