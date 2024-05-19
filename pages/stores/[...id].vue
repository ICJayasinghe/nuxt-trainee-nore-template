<template>
    <div>
        <InsideSection 
          page="stores"
          :category="category" 
          :endpointid="endpointid"
          :catpath="catpath"
          type=""
        />
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';


const route = useRoute();
const endpointid = ref('');
const category = ref('');
const catpath = ref('');

onMounted(async () => {
endpointid.value = route.params.id[0];
  if (endpointid.value === 'DRESSES') {
    category.value = 'Dresses';
    catpath.value = 'SHOP NOW, DRESSES'; 
  } else 
  if (endpointid.value === 'SKIRTS') {
    category.value = 'Skirts';
    catpath.value = 'SHOP NOW, SKIRT'; 
  } else 
  if (endpointid.value === 'JUMPSUITS & ROMPERS') {
    category.value = 'Jumpsuits & Rompers';
    catpath.value = 'SHOP NOW, JUMPSUIT'; 
  } else 
  if (endpointid.value === 'PANTS & SHORTS') {
    category.value = 'Pants & Shorts';
    catpath.value = 'SHOP NOW, DENIM'; 
  } else 
  if (endpointid.value === 'BLOUSES') {
    category.value = 'Blouses';
    catpath.value = 'SHOP NOW, TOPS'; 
  } else
  if (endpointid.value === 'KIDS WEAR') {
    category.value = 'Kids Wear';
    catpath.value = 'SHOP NOW, KIDS WEAR'; 
  }
});

console.log(endpointid.value)


</script>

