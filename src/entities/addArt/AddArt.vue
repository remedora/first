<template>
  <n-form ref="formRef" :rules="rules" :model="form">
    <n-form-item label="Наименование товара" path="artName" required>
      <n-input
        v-model:value="form.artName"
        placeholder="Введите наименование товара"
      />
    </n-form-item>

    <n-form-item label="Цена" path="price" required>
      <n-input v-model:value="form.price" placeholder="Укажите цену" />
    </n-form-item>

    <n-form-item label="Описание" path="description" required>
      <n-input
        v-model:value="form.description"
        placeholder="Введите описание товара"
      />
    </n-form-item>

    <n-form-item>
      <n-button type="primary" @click="handleSubmit">Отправить заказ</n-button>
      <n-button @click="closeForm" type="default">Закрыть</n-button>
    </n-form-item>
  </n-form>
</template>

<script setup lang="ts">
import { ref, defineEmits } from "vue";
import { NForm, NFormItem, NInput, NButton, useMessage } from "naive-ui";
import axios from "axios";
const message = useMessage();
const emit = defineEmits(["close"]);
const formRef = ref(null);

const form = ref({
  artName: "",
  price: null,
  description: "",
});

const rules = {
  artName: {
    required: true,
    message: "Обязательно для заполнения",
    trigger: ["blur"],
  },
  price: {
    required: true,
    message: "Обязательно для заполнения",
    trigger: ["blur"],
  },
  description: {
    required: true,
    message: "Обязательно для заполнения",
    trigger: ["blur"],
  },
};

const handleSubmit = () => {
  formRef.value?.validate(async (errors) => {
    if (!errors) {
      const { status } = await axios.post("https://httpbin.org/post");
      if (status === 200) {
        message.success("Товар успешно добавлен!");
      }
      emit("close");
    } else {
      message.warning("Пожалуйста, заполните форму.");
    }
  });
};

const closeForm = () => {
  emit("close");
};
</script>
