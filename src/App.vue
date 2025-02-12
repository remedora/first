<script setup lang="ts">
import { NMessageProvider } from "naive-ui";
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <NMessageProvider>
    <RouterView />
  </NMessageProvider>
</template>

<style scoped></style>
