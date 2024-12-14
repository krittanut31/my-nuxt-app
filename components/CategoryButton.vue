<template>
  <button
    type="button"
    class="btn btn-outline-primary text-dark border-primary border-3 d-flex justify-content-center align-items-center w-100 gap-md-2 gap-0"
    @click="handleClick"
  >
    <i class="material-icons text-info" style="font-size: 16px;" v-if="dataSelect?.includes(data)"
      >check_circle</i
    >
    <p class="name-cate">{{ data }}</p>
  </button>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import type { PropType } from "vue";

const props = defineProps({
  data: {
    type: String,
    required: true,
  },
  dataSelect: {
    type: Array as PropType<string[]>,
    required: true,
  },
});

const emit = defineEmits(["click"]);

const handleClick = () => {
  if (props.data) {
    emit("click", props.data);
  } else {
    console.error("Prop `data` is not defined.");
  }
};
</script>

<style scoped>
.name-cate {
  margin: 0;
  font-size: 13px;
}

@media screen and (min-width: 768px) {
  .name-cate {
    margin: 0;
    font-size: 16px;
  }
}

button:hover .name-cate {
  color: #fff;
}
</style>
