<template>
  <div class="bg-white text-black">
    <nav class="px-3 py-5 border-b-[1px] border-gray-100">
      <div class="flex container mx-auto items-center text-gray-500 font-bold text-xs space-x-1">
        <span>Home</span> <p>|</p>
        <span>{{ props.page }}</span> <p>|</p>
        <span>{{ props.category }}</span>
      </div>
    </nav>

    <div>
      <div class="flex space-x-4 pb-10 md:pb-20 pt-20 md:pt-12">
        <div class="relative">
          <button @click="toggleDropdown" class="bg-white border px-10 py-4 rounded flex items-center space-x-2">
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

        <UButton color="black" label="Filter" @click="isOpen = true" class="font-bold text-xs text-gray-700 border px-8 py-4 rounded"/>

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


        <UPagination class="flex justify-center mb-16"
        v-model="page"
        :page-count="24" 
        :max="5" 
        :total="items.length"
        :to="(page: number) => ({
            query: { page },
            hash: 'https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data',
          })"
        />
        
        <div class="border-t border-gray-200 w-full"></div>

      </section>
    </div>
  </div>

  <USlideover v-model="isOpen">
      <UCard class="flex flex-col flex-1 text-black" :ui="{ body: { base: 'flex-1 bg-white' }, }">
          <div class="flex justify-between items-center text-sm font-bold bg-white">
            <p>Filter</p>
            <svg @click="closeSlideover" class="w-7 h-7" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <g fill="currentColor" fill-rule="evenodd" clip-rule="evenodd">
                <path d="M5.47 5.47a.75.75 0 0 1 1.06 0l12 12a.75.75 0 1 1-1.06 1.06l-12-12a.75.75 0 0 1 0-1.06"/>
                <path d="M18.53 5.47a.75.75 0 0 1 0 1.06l-12 12a.75.75 0 0 1-1.06-1.06l12-12a.75.75 0 0 1 1.06 0"/>
              </g>
            </svg>
          </div>


          <div class="flex-grow overflow-y-auto overflow-x-hidden mb-10 mt-4 bg-white">
            <div class="flex items-center mb-4 space-x-3 justify-between">
              <p class="text-xs font-bold uppercase"> Categories </p>
            </div>
            <div class="grid grid-cols-2 gap-2 mt-6 text-xs font-bold text-center mb-8 md:mb-16">
              <NuxtLink to="/stores/DRESSES/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Dresses </NuxtLink>
              <NuxtLink to="/stores/SKIRTS/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Skirts </NuxtLink>
              <NuxtLink to="/stores/JUMPSUITS & ROMPERS/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Jumpsuits & Rompers </NuxtLink>
              <NuxtLink to="/stores/PANTS & SHORTS/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Pants & Shorts </NuxtLink>
              <NuxtLink to="/stores/BLOUSES/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Blouses </NuxtLink>
              <NuxtLink to="/stores/TEE SHIRTS/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Tee Shirts </NuxtLink>
            </div>
          </div>

          <div class="flex p-3 justify-center mt-80 md:mt-96 bg-white">
            <button @click="clearFilters" class="flex-grow p-3 bg-black text-white text-xs font-bold active:bg-white tracking-wider">Clear All Filters</button>
          </div>
      </UCard>
    </USlideover>
      
</template>

<script setup lang="ts">
import { ref, onMounted, watch, watchEffect } from 'vue';
import ProductCard from '~/components/ProductCard.vue';

const page = ref(1)
const items = ref(Array(240))

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

const isOpen = ref(false);

function closeSlideover() {
  isOpen.value = false;
}

function clearFilters() {
  // Your clear filters logic here
  closeSlideover();
}
</script>


<style scoped>
.slide-right-enter-active, .slide-right-leave-active {
  transition: transform 0.8s ease-in-out;
}
.slide-right-enter, .slide-right-leave-to {
  transform: translateX(100%);
}
</style>