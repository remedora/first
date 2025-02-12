<template>
  <n-card v-for="(item, index) in items" :key="index" :title="item.title">{{
    item.description
  }}</n-card>
</template>
<script setup lang="ts">
import axios from "axios";
import { ref } from "vue";
import { NCard } from "naive-ui";
const items = ref([]);
const getCard = async () => {
  const { data } = await axios.get("https://fakestoreapi.com/products");
  items.value = data;
};
getCard();
</script>
