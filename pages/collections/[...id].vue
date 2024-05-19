<template>
  <div>
    <InsideSection 
      page="collections"
      :category="category" 
      :endpointid="endpointid" 
      :type="type"
      catpath=""
      orderMapping=""
    />

  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import InsideSection from '../../components/InsideSection.vue';


const route = useRoute();
const endpointid = ref('');

const category = ref('');
const type = ref('');

onMounted(async () => {
  endpointid.value = route.params.id[0];
  try {
    if (endpointid.value === 'NewArrivals') {
      category.value = 'New Arrivals';
      type.value = 'new_arrivals'; 
    } else if (endpointid.value === 'Offers') {
      category.value = 'Offers';
      type.value = 'hot_offer'; 
    } else if (endpointid.value === 'trends') {
      category.value = 'trends';
      type.value = 'trends'; 
    }
  } catch (error) {
    console.error('Error getting route path:', error);
  }
});

</script>

