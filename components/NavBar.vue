<template>
    <nav :class="[`px-3 py-4 w-full hidden lg:block bg-white sticky top-0 left-0 z-40 border-b border-gray-100`, isScrolled ? 'shadow-md' : ' shadow-none']">
      <div class="bg-white h-12 flex justify-between items-center">

        <NuxtLink to="/">
          <img src="~/assets/logo.png" alt="Logo" class="h-20 w-auto"/>
        </NuxtLink>

        <div>
          <ul class="flex space-x-5 mx-5">
            <NuxtLink to="/collections/NewArrivals/" class="text-black text-xs font-bold">NEW ARRIVALS</NuxtLink>
            <NavBarCard
              v-for="category in variables.categories"
              :key="category.id"
              :category="category"
              :selectedCategory="selectedCategory"
            />
            <NuxtLink to="/collections/Offers/" class="text-black text-xs font-bold">SALE</NuxtLink>
          </ul>
        </div>

      </div>
    </nav>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

interface Category {
  name: string;
  id: string;
  parent_id: string;
  img1: string;
  img2: string;
  children: Category[];
  order_by: number;
  link: string;
}

const variables = ref<{ categories: Category[] }>({ categories: [] });

const getCategories = async () => {
  try {
    const response = await fetch('https://api.dilferclothing.com/dilferclothing.com/get_product_category_list');
    const data = await response.json();

    let _categories: Category[] = [];
    for (let index = 0; index < data.length; index++) {
      if (data[index].parent_id === '-') {
        let _menu: Category = {
          name: data[index].name,
          id: data[index].id,
          parent_id: data[index].parent_id,
          img1: data[index].img_url1,
          img2: data[index].img_url2,
          children: [],
          order_by: data[index].order_by,
          link: `/categories/${data[index].name.replaceAll(' ', '-')}/`
        };
        _categories.push(_menu);
      } else {
        for (let j = 0; j < _categories.length; j++) {
          if (_categories[j].id === data[index].parent_id) {
            let _menu: Category = {
              name: data[index].name,
              id: data[index].id,
              parent_id: data[index].parent_id,
              img1: data[index].img_url1,
              img2: data[index].img_url2,
              children: [],
              order_by: data[index].order_by,
              link: `/categories/${_categories[j].name.replaceAll(' ', '-')}/${data[index].name.replaceAll(' ', '-')}/`
            };
            _categories[j].children.push(_menu);
          }
          for (let k = 0; k < _categories[j].children.length; k++) {
            if (_categories[j].children[k].id == data[index].parent_id) {
              let _menu = {
                name: data[index].name,
                id: data[index].id,
                parent_id: data[index].parent_id,
                img1: data[index].img_url1,
                img2: data[index].img_url2,
                children: [],
                order_by: data[index].order_by,
                link: `/categories/${_categories[j].name.replaceAll(' ', '-')}/${_categories[j].children[k].name.replaceAll(' ', '-')}/${data[index].name.replaceAll(' ', '-')}/`
              }
              _categories[j].children[k].children.push(_menu)
            }
          }
          _categories[j].children.sort((a: any, b: any) => a.order_by - b.order_by);
        }
      }
    }

    _categories.sort((a, b) => a.order_by - b.order_by);
    variables.value.categories = _categories;
  } catch (error) {
    console.error('Error fetching categories:', error);
  }
};

const selectedCategory = ref<Category | null>(null);
const isScrolled = ref(false);

const handleScroll = () => {
  isScrolled.value = window.scrollY > 0;
};

onMounted(() => {
  getCategories();
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});


</script>
