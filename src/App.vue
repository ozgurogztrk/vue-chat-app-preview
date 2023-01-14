<script setup lang="ts">
import TheHeader from "./components/TheHeader.vue";
import TheFooter from "./components/TheFooter.vue";
</script>

<template>
  <div class="flex flex-column align-center justify-center">
    <TheHeader />

    <Transition>
      <RouterView />
    </Transition>

    <TheFooter />
  </div>
</template>

<style scoped lang="scss">
@use "./assets/scss/base/transition";
</style>
