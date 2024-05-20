<template>
    <div>
      <ProductDetail :product="product" />
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted } from 'vue';
  import ProductDetail from '../../components/ProductDetail.vue';
  import { useRoute } from 'vue-router';
  
  const route = useRoute();
  const product = ref(null);
  
  const fetchProduct = async () => {
    try {
      const response = await fetch(`https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data/${route.params.id}`);
      product.value = await response.json();
    } catch (error) {
      console.error('Error fetching product:', error);
    }
  };
  
  onMounted(fetchProduct);
</script>