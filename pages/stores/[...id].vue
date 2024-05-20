<template>
  <div>
    <InsideSection 
      page="stores"
      :category="category" 
      :endpointid="endpointid"
      :catpath="catpath"
      type=""
      orderMapping=""
      :key="uniqueKey"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from 'vue';
import { useRoute } from 'vue-router';
import InsideSection from '../../components/InsideSection.vue';

const route = useRoute();
const endpointid = ref('');
const category = ref('');
const catpath = ref('');
const uniqueKey = ref(0);  

const updateState = () => {
  endpointid.value = route.params.id[0] || '';
  switch (endpointid.value) {
    case 'DRESSES':
      category.value = 'Dresses';
      catpath.value = 'SHOP NOW, DRESSES';
      break;
    case 'SKIRTS':
      category.value = 'Skirts';
      catpath.value = 'SHOP NOW, SKIRT';
      break;
    case 'JUMPSUITS & ROMPERS':
      category.value = 'Jumpsuits & Rompers';
      catpath.value = 'SHOP NOW, JUMPSUIT';
      break;
    case 'PANTS & SHORTS':
      category.value = 'Pants & Shorts';
      catpath.value = 'SHOP NOW, DENIM';
      break;
    case 'BLOUSES':
      category.value = 'Blouses';
      catpath.value = 'SHOP NOW, TOPS';
      break;
    case 'KIDS WEAR':
      category.value = 'Kids Wear';
      catpath.value = 'SHOP NOW, KIDS WEAR';
      break;
    default:
      category.value = '';
      catpath.value = '';
      break;
  }
  uniqueKey.value++; 
};

watchEffect(() => {
  updateState();
});

</script>
