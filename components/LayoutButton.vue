<template>
  <button
    type="button"
    class="btn btn-outline-primary text-dark border-primary border-3 w-100 d-flex justify-content-center align-items-center"
    @click="handleClick"
  >
    <i class="material-icons text-info">{{ data.icon }}</i>
    <p class="name-layout">{{ data?.name }}</p>
  </button>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import type { PropType } from "vue";
import type { layoutType } from "~/types";

const props = defineProps({
  data: {
    type: Object as PropType<layoutType>,
    required: true,
  },
  dataSelect: {
    type: Object as PropType<layoutType | null>,
    required: true,
  },
});

const emit = defineEmits(["click"]);

const handleClick = () => {
  if (props.data) {
    emit("click", props.data); // ส่ง prop `data` กลับไปยัง Parent Component
  } else {
    console.error("Prop `data` is not defined.");
  }
};
</script>

<style scoped>
.name-layout{
  margin: 0;
  font-size: 13px;
}

@media screen and (min-width: 768px) {
  .name-layout {
    margin: 0;
    font-size: 16px;
  }
}

button:hover .name-layout{
  color:#fff
}
</style>
