<template>
  <div class="style-image position-relative w-100">
    <img :src="data.imageUrl" :alt="data.name" />
    <div class="image-name ">
      {{ data.name }}
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import type { styleImageType } from "~/types";
import type {} from "vue";

const props = defineProps({
  data: {
    type: Object as PropType<styleImageType>,
    required: true,
  },
});
</script>

<style>
.style-image {
  aspect-ratio: 1/1;
  overflow: hidden;
}

.style-image img {
  aspect-ratio: 1/1;
  object-fit: cover;
  width: 100%;
}

.image-name{
  -webkit-line-clamp: 1;
  text-overflow: ellipsis;
  background-color: #ffffff50;
  position: absolute;
  bottom: 0;
  color:#000;
  text-align: center;
  width: 100%;
}
</style>
