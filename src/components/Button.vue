<script setup lang="ts">
import { defineEmits, VueElement } from "vue";

defineProps({
  type: {
    type: String,
    required: true,
  },
  content: {
    type: String || Number,
    required: true,
  },
});

const emits = defineEmits(["on-click"]);

const onClick = () => {
  emits("on-click");
};
</script>

<template>
  <button @click="onClick" :class="type">
    {{ content }}
  </button>
</template>

<style scoped lang="scss">
@use "../assets/scss/components/button";
</style>
