<template>
  <div>
    <InsideSection 
      :page="page"
      :category="category" 
      :endpointid="endpointid" 
      :type="type"
      catpath=""
      orderMapping=""
      :key="uniqueKey"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';
import { useRoute } from 'vue-router';
import InsideSection from '../../components/InsideSection.vue';

const route = useRoute();
const endpointid = ref('');
const category = ref('');
const type = ref('');
const page = 'collections';
const uniqueKey = ref(0);

const updateState = () => {
  endpointid.value = route.params.id[0] || '';

  if (endpointid.value === 'NewArrivals') {
    category.value = 'New Arrivals';
    type.value = 'new_arrivals'; 
  } else if (endpointid.value === 'Offers') {
    category.value = 'Offers';
    type.value = 'hot_offer'; 
  } else if (endpointid.value === 'trends') {
    category.value = 'Trends';
    type.value = 'trends'; 
  } else {
    category.value = '';
    type.value = '';
  }
  uniqueKey.value++; 
};

onMounted(updateState);

watch(
  () => route.params.id,
  () => {
    updateState();
  }
);
</script>
