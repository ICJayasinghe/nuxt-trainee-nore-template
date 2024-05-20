<template>
  <div class="bg-white">
    <nav class="px-3 py-5 border-b-[1px] border-gray-100">
      <div class="flex container mx-auto items-center text-gray-500 font-bold text-xs space-x-1">
        <span>Home</span> <p>|</p>
        <span>{{ props.page }}</span> <p>|</p>
        <span>{{ props.category }}</span>
      </div>
    </nav>

    <div class="py-12">
      <div class="flex space-x-4 pb-10 md:pb-20 pl-5">
        <div class="relative">
          <button @click="toggleDropdown" class="bg-white border px-8 py-4 rounded flex items-center space-x-2">
            <span class="font-bold text-xs text-gray-700">{{ selectedSort }}</span>
            <svg class="w-4 h-4" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 9l6 6 6-6"/>
            </svg>
          </button>
          <div v-if="dropdownOpen" class="absolute mt-1 bg-white border rounded shadow-lg w-full z-10">
            <button v-for="option in sortOptions" :key="option" @click="selectSort(option)" class="block w-full text-xs text-left px-4 py-2 hover:bg-blue-600">
              {{ option }}
            </button>
          </div>
        </div>

        <button @click="toggleFilter" class="font-bold text-xs text-gray-700 border px-8 py-4 rounded">
          Filter
        </button>
      </div>

      <section>
        <div class="container mx-auto grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
          <ProductCard 
            v-for="product in products" 
            :key="product.id" 
            :src="product.img" 
            :name="product.master_title" 
            :price="`LKR ${(product.PRICE - product.DISCOUNT)}.00`" 
            :discount="props.catpath === '' && product.DISCOUNT ? `LKR ${(product.PRICE).toFixed(2)}.00` : ''"
            :installment="`LKR ${((product.PRICE - product.DISCOUNT) / 3).toFixed(2)} x 3 with`"
            :discountPercentage="product.PRICE > 0 ? Math.round((product.DISCOUNT / product.PRICE) * 100) : 0" 
          />
        </div>

        <div class="flex items-center justify-center mt-8 pb-14 md:pb-12">
          <div class="flex items-center space-x-2">
            <button>
              <svg class="w-6 h-6" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24">
                <path fill="currentColor" d="M18.29 17.29a.996.996 0 0 0 0-1.41L14.42 12l3.88-3.88a.996.996 0 1 0-1.41-1.41L12.3 11.3a.996.996 0 0 0 0 1.41l4.59 4.59c.38.38 1.01.38 1.4-.01"/><path fill="currentColor" d="M11.7 17.29a.996.996 0 0 0 0-1.41L7.83 12l3.88-3.88a.996.996 0 1 0-1.41-1.41L5.71 11.3a.996.996 0 0 0 0 1.41l4.59 4.59c.38.38 1.01.38 1.4-.01"/>
              </svg>
            </button>
            <button class="aspect-square w-8 border-[1px] bg-black text-white duration-300"> 1 </button>
            <button class="aspect-square w-8 border-[1px] bg-white text-black hover:bg-black hover:text-white duration-300"> 2 </button>
            <button>
              <svg class="w-6 h-6" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24">
                <path fill="currentColor" d="M9.575 12L5 7.4L6.4 6l6 6l-6 6L5 16.6zm6.6 0L11.6 7.4L13 6l6 6l-6 6l-1.4-1.4z"/>
              </svg>
            </button>
          </div>
        </div>
        <div class="border-t border-gray-200 w-full"></div>
      </section>
    </div>
  </div>

  <transition name="slide-right">
    <FilterOption v-if="isFilterOn" @close="isFilterOn = false"/>
  </transition>
</template>

<script setup lang="ts">
import { ref, onMounted, watch, watchEffect } from 'vue';
import ProductCard from '~/components/ProductCard.vue';

type Product = {
  id: number;
  sku: string;
  PRICE: number;
  DISCOUNT: number;
  master_title: string;
  img: string;
  img2: string;
};

const props = defineProps<{
  page: string;
  category: string;
  endpointid: string;
  type: string;
  catpath: string;
  orderMapping: string;
}>();

const products = ref<Product[]>([]);
const dropdownOpen = ref(false);
const selectedSort = ref('Newest');
const sortOptions = ['Newest', 'Popularity', 'Price High to Low', 'Price Low to High'];

const toggleDropdown = () => {
  dropdownOpen.value = !dropdownOpen.value;
};

const selectSort = (option: string) => {
  selectedSort.value = option;
  dropdownOpen.value = false;
  fetchProducts(); // Fetch products again with the new sorting option
};

const fetchProducts = async () => {
  try {
    const orderMapping: { [key: string]: string } = {
      'Newest': 'NEWEST',
      'Popularity': 'POPULARITY',
      'Price High to Low': 'PRICE_HIGH_TO_LOW',
      'Price Low to High': 'PRICE_LOW_TO_HIGH',
    };

    const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
      },
      body: JSON.stringify({
        currency_code: 'LKR',
        is_master: true,
        p: 1,
        size: 24,
        order: orderMapping[selectedSort.value],
        maxprice: 1000000,
        minprice: 0,
        brand: "",
        type: props.type || '',
        cat_path: props.catpath || '',
      }),
    });

    const data = await response.json();

    products.value = data.list.map((product: any) => {
      const imgUrl = JSON.parse(product.variants).reduce((acc: string[], variant: any) => {
        if (variant.product_img_urls) {
          acc.push(...variant.product_img_urls.map((img: any) => img.main_image));
        }
        return acc;
      }, []);

      return {
        ...product,
        name: product.master_title,
        img: imgUrl.length > 0 ? imgUrl[0] : '',
      };
    });
  } catch (error) {
    console.error('Error fetching products:', error);
  }
  return products;
};

// Fetch products on component mount and watch for changes in props or sorting
onMounted(fetchProducts);
watch(() => [props.type, props.catpath, selectedSort.value], fetchProducts);

// Watch the endpointid to ensure the component re-renders on route change
watchEffect(() => {
  fetchProducts();
});

const isFilterOn = ref(false);

const toggleFilter = () => {
  isFilterOn.value = !isFilterOn.value;
};
</script>

<style scoped>
.slide-right-enter-active, .slide-right-leave-active {
  transition: transform 0.8s ease-in-out;
}
.slide-right-enter, .slide-right-leave-to {
  transform: translateX(100%);
}
</style>
