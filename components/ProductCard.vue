<template>
  <NuxtLink :to="`/products/${id}`" class="relative px-3 text-xs font-bold">
    <img :src="src" alt="product-img" class="w-full">
    <div v-if="discountPercentage" class="absolute top-2 right-5 bg-red-500 text-white px-2 py-3 rounded-full">
      -{{ discountPercentage }}%
    </div>
    <div class="p-3 space-y-2">
      <p>{{ name }}</p>
      <div class="flex space-x-3">
        <p>{{ price }}</p>
        <p class="text-gray-500 line-through">{{ discount }}</p>
      </div>
      <div class="flex space-x-3 pb-8">
        <p class="font-bold text-gray-500">{{ installment }}</p>
        <img class="w-12" alt="intpay" src="https://greencoding.b-cdn.net/norareedfashion.com/front-end-asseta/mp.svg">
      </div>
    </div>
  </NuxtLink>
</template>

<script setup lang="ts">
const props = defineProps({
  src: String,
  name: String,
  price: String,
  discount: String,
  installment: String,
  discountPercentage: Number,
  id: String, // Add id prop for navigation
});
</script>
