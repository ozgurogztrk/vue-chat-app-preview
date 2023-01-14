<script setup lang="ts">
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { RouterLink } from "vue-router";
</script>

<template>
  <div class="flex align-center justify-center gap-column w-100 card">
    <RouterLink
      to="/"
      class="button-navigation"
      exactActiveClass="button-navigation active"
    >
      <font-awesome-icon icon="fa fa-right-to-bracket" />
    </RouterLink>

    <RouterLink
      to="/chat"
      class="button-navigation"
      exactActiveClass="button-navigation active"
    >
      <font-awesome-icon icon="fa fa-message" />
    </RouterLink>

    <RouterLink
      to="/account"
      class="button-navigation"
      exactActiveClass="button-navigation active"
    >
      <font-awesome-icon icon="fa fa-user" />
    </RouterLink>
  </div>
</template>

<style scoped lang="scss">
@use "../assets/scss/components/button";
</style>
