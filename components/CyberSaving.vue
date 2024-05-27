<template>
  <div class="w-full bg-white text-black">
    <div class="border-b border-gray-300 mb-8"></div>
    <div class="my-8">
      <p class="font-bold text-2xl md:text-5xl uppercase text-center">CYBER SAVINGS</p>
    </div>
    <div class="border-b border-gray-300 mb-12"></div>
    <div class="grid grid-cols-2 md:grid-cols-4">
      <ProductCard 
        v-for="product in products"
        :key="product.id"
        :src="product.img"
        :name="product.master_title"
        :price="`LKR ${(product.PRICE - product.DISCOUNT)}.00`"
        :discount="`LKR ${product.PRICE}.00`"
        :installment="`LKR ${((product.PRICE - product.DISCOUNT) / 3).toFixed(2)} x 3 with`"
        :discountPercentage="product.PRICE > 0 ? Math.round((product.DISCOUNT / product.PRICE) * 100) : 0"
      />
    </div>

    <div class="flex justify-end items-center mx-5 mt-5">
      <NuxtLink to="/collections/Offers/" class="bg-black text-white py-3 px-12 text-xs mb-12">View All</NuxtLink>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import ProductCard from '~/components/ProductCard.vue';

type Product = {
  id: number;
  img: string;
  master_title: string;
  PRICE: number;
  DISCOUNT: number;
};

const products = ref<Product[]>([]);

const fetchProducts = async () => {
  try {
    const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        currency_code: 'LKR',
        is_master: true,
        item_size: "",
        maxprice: 1000000,
        minprice: 0,
        order: 'NEWEST',
        p: 1,
        size: 8,
        type: 'hot_offer',
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
        master_title: product.master_title,
        img: imgUrl.length > 0 ? imgUrl[0] : '',
      };
    });

  } catch (error) {
    console.error('Error fetching products:', error);
  }

  return products;
};

onMounted(fetchProducts);
</script>
