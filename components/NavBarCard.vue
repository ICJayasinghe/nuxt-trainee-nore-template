<template>
    <div>
      <li @click="handleClick">
        <NuxtLink :to="categoryLink" class="text-black text-xs font-bold">
          <p>{{ category.name }}</p>
        </NuxtLink>
      </li>
    </div>
</template>


<script setup lang="ts">

const props = defineProps<{
  category: {
    name: string;
    id: string;
    parent_id: string;
    img1: string;
    img2: string;
    children: any[];
    order_by: number;
    link: string;
  };
  selectedCategory: any;
}>();

const categoryLink = computed(() => {
  return `/stores/${props.category.name}`;
});


const emit = defineEmits(['close']);

const handleClick = () => {
  emit('close');
};

</script>


