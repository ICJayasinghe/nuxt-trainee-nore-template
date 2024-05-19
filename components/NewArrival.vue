<template>
    <div>
      <div class="my-8 w-full">
        <p class="font-bold text-2xl md:text-5xl uppercase text-center">New Arrivals</p>
      </div>
      <div class="border-b border-gray-300 mb-12"></div>
      <div class=" grid grid-cols-2 md:grid-cols-4">

          <ProductCard 
            v-for="product in products"
            :key="product.id"
            :src="product.img"
            :name="product.master_title"
            :price="`LKR ${product.PRICE}.00`"
            :installment="`LKR ${(product.PRICE/3).toFixed(2)} x 3 with`"
          />

      </div>

      <div class="flex justify-end items-center mx-5 mt-5">
        <NuxtLink to="/collections/NewArrivals/" class="bg-black text-white py-3 px-12 text-xs mb-12">View All</NuxtLink>
      </div>

    </div>
  </template>
  
<script setup lang="ts">

  type Product = {
    id: number;
    img: string;
    master_title: string;
    PRICE: number;
  };
  
  const products = ref<Product[]>([]);
  
  const fetchproducts = async () =>{
    try{
      const response = await fetch('https://gcp-store-shared1.greencloudpos.com/norareedfashion.com/store_data',{
  
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
          type: 'new_arrivals',
        }),
  
      });
  
      const data = await response.json();
      products.value = data.list.map((product: any) => {
        const imgUrl = JSON.parse(product.variants).reduce((acc: string[], variant: any) => {
          if (variant.product_img_urls){
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
  
    }catch(error){
      console.error('Error fetching products:', error)
    }
  
    return products;
  }
  
  onMounted(fetchproducts);
  
</script>