<template>
    <div class="flex fixed lg:hidden inset-0 z-50 h-full w-full opacity-100 pointer-events-auto">
      <div class="absolute inset-0 bg-black bg-opacity-75 h-full w-full opacity-100 pointer-events-auto" @click="emit('close')"></div>
      <nav class="absolute top-0 bg-white h-full w-full flex flex-col max-w-md left-0 delay-300 duration-300">
        <div class="pr-3 border-b flex items-center justify-between h-16">
          <NuxtLink to="/" @click="emit('close')">
            <img alt="site-logo" class="h-8 px-3" src="~/assets/logo.png">
          </NuxtLink>
          <button @click="emit('close')">
            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 21 21">
              <path fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" d="m15.5 15.5l-10-10zm0-10l-10 10"/>
            </svg>
          </button>
        </div>
  
        <div class="flex-grow mt-6 px-3 overflow-y-auto">
          <ul class="flex flex-col space-y-4 list-inside font-bold text-xs tracking-wide">
            <NuxtLink to="/collections/NewArrivals/" @click="emit('close')">
              <li class="border-b pb-4 px-4">NEW ARRIVALS</li>
            </NuxtLink>
                <NavBarCard
                  v-for="category in variables.categories"
                  class="border-b pb-4 px-4"
                  :key="category.id"
                  :category="category"
                  selectedCategory=" "
                  @click="emit('close')"
                />
            <NuxtLink to="/collections/Offers/" @click="emit('close')">
              <li class="border-b pb-4 px-4">SALE</li>
            </NuxtLink>
          </ul>
        </div>
      </nav>
    </div>
  </template>


<script setup lang="ts">

const emit = defineEmits(['close']);

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

const variables = reactive<{ categories: Category[] }>({
  categories: []
});


const get_categories = async () => {
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
                        if(_categories[j].children[k].id == data[index].parent_id){
                            let _menu = {
                                name : data[index].name,
                                id : data[index].id,
                                parent_id : data[index].parent_id,
                                img1 : data[index].img_url1,
                                img2 : data[index].img_url2,
                                children : new Array,
                                order_by : data[index].order_by,
                                link : `/categories/${_categories[j].name.replaceAll(' ', '-')}/${_categories[j].children[k].name.replaceAll(' ', '-')}/${data[index].name.replaceAll(' ', '-')}/`
                            }
                            
                            _categories[j].children[k].children.push(_menu)
                  
                        }                            
                    }                
                    _categories[j].children.sort((a:any,b:any) => a.order_by - b.order_by);
                }
            }
        }

        _categories.sort((a, b) => a.order_by - b.order_by);
        variables.categories = _categories;
    } catch (error) {
        console.error('Error fetching categories:', error);
    }
};

onMounted(get_categories);

</script>

<style scoped>

.sticky {
  position: -webkit-sticky;
  position: sticky;
  top: 0;
  z-index: 50;
}

</style>