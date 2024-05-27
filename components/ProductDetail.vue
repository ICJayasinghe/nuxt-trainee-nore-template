<template>
  <div class="container mx-auto px-4 py-8">
    <div v-if="productLoaded" class="flex flex-col md:flex-row">
      <!-- Product Image Gallery -->
      <div class="w-full md:w-1/2">
        <div class="flex flex-col space-y-2">
          <img :src="mainImage" alt="Main Image" class="w-full h-auto rounded">
          <div class="flex space-x-2">
            <img
              v-for="(image, index) in product.images"
              :key="index"
              :src="image"
              alt="Thumbnail"
              class="w-20 h-20 rounded cursor-pointer"
              @click="mainImage = image"
            >
          </div>
        </div>
      </div>

      <!-- Product Details -->
      <div class="w-full md:w-1/2 md:pl-10 mt-8 md:mt-0">
        <h1 class="text-2xl font-bold mb-4">{{ product.name }}</h1>
        <div class="text-xl font-semibold mb-2">{{ formattedPrice }}</div>
        <div class="text-sm mb-4">{{ formattedInstallment }}</div>
        <div class="mb-4">
          <div class="text-sm text-gray-500">Code: {{ product.code }}</div>
          <div class="text-sm text-orange-500">Availability: {{ product.availability }}</div>
        </div>
        <div class="text-sm mb-4">
          <div>Fabric: {{ product.fabric }}</div>
          <div>Colour: {{ product.color }}</div>
        </div>

        <!-- Size Selection -->
        <div class="mb-6">
          <div class="text-sm mb-2">Size:</div>
          <div class="flex space-x-2">
            <button
              v-for="size in product.sizes"
              :key="size"
              @click="selectedSize = size"
              :class="['px-4 py-2 border rounded', selectedSize === size ? 'bg-black text-white' : 'bg-white text-black']"
            >
              {{ size }}
            </button>
          </div>
        </div>

        <!-- Quantity and Add to Cart -->
        <div class="flex items-center space-x-4 mb-4">
          <div class="flex items-center border rounded">
            <button @click="decreaseQuantity" class="px-2 py-1">-</button>
            <input type="text" v-model="quantity" class="w-12 text-center border-none focus:ring-0">
            <button @click="increaseQuantity" class="px-2 py-1">+</button>
          </div>
          <button class="px-8 py-2 bg-black text-white rounded">Add To Cart</button>
        </div>

        <button class="px-8 py-2 border rounded text-black"></button>

        <!-- Additional Information Sections -->
        <div class="mt-8">
          <details class="mb-4">
            <summary class="font-bold">Product Details</summary>
            <div class="mt-2 text-sm">
              <div>Brand: {{ product.brand }}</div>
              <div>Code: {{ product.code }}</div>
              <div>Availability: {{ product.availability }}</div>
            </div>
          </details>

          <details class="mb-4">
            <summary class="font-bold">Delivery Details</summary>
            <div class="mt-2 text-sm">
              Delivery information goes here.
            </div>
          </details>

          <details>
            <summary class="font-bold">Size Guide</summary>
            <div class="mt-2 text-sm">
              Size guide information goes here.
            </div>
          </details>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue';

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

const props = defineProps<{ product: Product }>();

// Reactive state for the main image and selected size
const mainImage = ref('');
const selectedSize = ref('');
const quantity = ref(1);
const productLoaded = ref(false);

// Watch the product prop and initialize the state
watch(
  () => props.product,
  (newProduct) => {
    if (newProduct && newProduct.images && newProduct.images.length > 0) {
      mainImage.value = newProduct.images[0];
      selectedSize.value = newProduct.sizes[0];
      productLoaded.value = true;
    }
  },
  { immediate: true }
);

// Functions to increase and decrease quantity
const decreaseQuantity = () => {
  if (quantity.value > 1) quantity.value--;
};

const increaseQuantity = () => {
  quantity.value++;
};

// Computed properties for formatted price and installment
const formattedPrice = computed(() => `LKR ${props.product.price}.00`);
const formattedInstallment = computed(() => `LKR ${props.product.installment} x 3 with Mintpay`);
</script>

