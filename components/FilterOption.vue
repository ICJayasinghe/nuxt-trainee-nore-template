<template>
    <div class="fixed inset-0 h-full w-full z-50 duration-300 opacity-100 pointer-events-auto">
        <div 
            class="absolute bg-black inset-0 h-full w-full bg-opacity-40 backdrop-blur-md  duration-300 opacity-100 pointer-events-auto"
            @click="emit('close')">
        </div>
  
    <div class="absolute bg-white top-0 right-0 h-full w-full max-w-lg delay-300 duration-300 overflow-x-hidden" >
        <div class="flex p-2 justify-between items-center text-sm font-bold border-b-[1px]">
            <p>Filter</p>
            <svg @click="emit('close')" class="w-7 h-7"  xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><g fill="currentColor" fill-rule="evenodd" clip-rule="evenodd">
                <path d="M5.47 5.47a.75.75 0 0 1 1.06 0l12 12a.75.75 0 1 1-1.06 1.06l-12-12a.75.75 0 0 1 0-1.06"/><path d="M18.53 5.47a.75.75 0 0 1 0 1.06l-12 12a.75.75 0 0 1-1.06-1.06l12-12a.75.75 0 0 1 1.06 0"/></g>
            </svg>
        </div>
  
        <div class="flex-grow overflow-y-auto overflow-x-hidden">
            <div class="mb-10 mt-4 px-3">
                <div class="flex items-center mb-4 space-x-3 justify-between">
                    <p class="text-xs font-bold uppercase"> Categories </p>
                </div>
                <div class="grid grid-cols-2 gap-2 ml-4 mt-6 text-xs font-bold text-center">
                    <NuxtLink to="/stores/DRESSES/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Dresses </NuxtLink>
                    <NuxtLink to="/stores/SKIRTS/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Skirts </NuxtLink>
                    <NuxtLink to="/stores/JUMPSUITS & ROMPERS/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Jumsuits & Rompers </NuxtLink>
                    <NuxtLink to="/stores/PANTS & SHORTS/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Pants & Shorts </NuxtLink>
                    <NuxtLink to="/stores/BLOUSES/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Blouses </NuxtLink>
                    <NuxtLink to="/stores/TEE SHIRTS/" class="duration-300 hover:bg-black hover:text-white py-2 border border-gray-100"> Tee Shirts </NuxtLink>
                </div>
            </div>
        </div>
  
        <div class="flex p-3 justify-center pt-80 md:pt-96">
            <button @click="emit('close')"  class="flex-grow p-3 bg-black text-white text-xs font-bold active:bg-white tracking-wider">Clear All Filters</button>
        </div>
    </div>
    </div>
  </template>
  
  <script setup lang="ts">
  
  const emit = defineEmits(['close']);
  
  </script>