<template>
  <n-input v-model:value="search" placeholder="Наименование" />
  <n-input v-model:value="searchPrice" placeholder="Цена" />
  <n-button @click="showModalOrder">Открыть форму заказа</n-button>
  <n-modal
    v-model:show="isModalVisibleOrder"
    title="Форма заказа"
    preset="card"
    @close="resetFormOrder"
  >
    <OrderForm @close="closeModalOrder" />
  </n-modal>
  <n-button @click="showModalArt">Открыть форму добавления товара</n-button>
  <n-modal
    v-model:show="isModalVisibleArt"
    title="Форма добавления товара"
    preset="card"
    @close="resetFormArt"
  >
    <AddArt @close="closeModalArt" />
  </n-modal>

  <n-card v-for="(item, index) in filterList" :key="index" :title="item.title">
    <div class="price">
      <h2>Цена:</h2>
      <div>{{ item.price }}</div>
    </div>
    <div class="description">
      <h2>Описание:</h2>
      <div>{{ item.description }}</div>
    </div>
  </n-card>
</template>
<script setup lang="ts">
import axios from "axios";
import { OrderForm } from "@/entities/order";
import { AddArt } from "@/entities/addArt";
import { computed, ref } from "vue";
import { NCard, NModal, NButton, NInput } from "naive-ui";
const items = ref([]);
const getCard = async () => {
  const { data } = await axios.get("https://fakestoreapi.com/products");
  items.value = data;
};
const search = ref("");
const searchPrice = ref("");
const filterList = computed(() => {
  let list = items.value;
  if (search.value) {
    list = items.value.filter((item) => item.title.includes(search.value));
    // return items.value
    //   .filter((item) => item.title.includes(search.value))
    //   .filter((item) => item.price == searchPrice.value);
  }
  if (searchPrice.value) {
    list = items.value.filter((item) => item.price == searchPrice.value);
  }
  return list;
});
getCard();
const isModalVisibleArt = ref(false);
const isModalVisibleOrder = ref(false);

const showModalArt = () => {
  isModalVisibleArt.value = true;
};
const showModalOrder = () => {
  isModalVisibleOrder.value = true;
};
const closeModalArt = () => {
  isModalVisibleArt.value = false;
};
const closeModalOrder = () => {
  isModalVisibleOrder.value = false;
};

const resetFormOrder = () => {
  closeModalOrder();
};
const resetFormArt = () => {
  closeModalArt();
};
</script>
