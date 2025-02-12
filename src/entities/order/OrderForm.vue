<template>
  <n-form ref="formRef" :rules="rules" :model="form">
    <n-form-item label="ФИО" path="fullName" required>
      <n-input v-model:value="form.fullName" placeholder="Введите ваше ФИО" />
    </n-form-item>

    <n-form-item label="Дата рождения" path="birthDate" required>
      <n-date-picker
        v-model:value="form.birthDate"
        placeholder="Выберите дату"
      />
    </n-form-item>

    <n-form-item label="Номер карты" path="cardNumber" required>
      <n-input
        v-model:value="form.cardNumber"
        placeholder="Введите номер карты"
      />
    </n-form-item>

    <n-form-item>
      <n-checkbox v-model:checked="form.termsAccepted">
        Я принимаю <a href="#">соглашение с правилами обработки заказа</a>
      </n-checkbox>
    </n-form-item>

    <n-form-item>
      <n-button type="primary" @click="handleSubmit">Отправить заказ</n-button>
      <n-button @click="closeForm" type="default">Закрыть</n-button>
    </n-form-item>
  </n-form>
</template>

<script setup lang="ts">
import { ref, defineEmits } from "vue";
import {
  NForm,
  NFormItem,
  NInput,
  NDatePicker,
  NCheckbox,
  NButton,
  useMessage,
} from "naive-ui";
import axios from "axios";
const message = useMessage();
const emit = defineEmits(["close"]);
const formRef = ref(null);

const form = ref({
  fullName: "",
  birthDate: null,
  cardNumber: "",
  termsAccepted: false,
});

const rules = {
  fullName: {
    required: true,
    message: "ФИО обязательно для заполнения",
    trigger: ["blur"],
  },
  birthDate: {
    required: true,
    validator: (rule, value) => {
      if (!value) {
        return new Error("Дата рождения обязательна для заполнения");
      }
      return true;
    },
    trigger: ["blur"],
  },
  cardNumber: {
    required: true,
    message: "Номер карты обязателен для заполнения",
    trigger: ["blur"],
  },
};

const handleSubmit = () => {
  formRef.value?.validate(async (errors) => {
    if (!errors) {
      const { status } = await axios.post("https://httpbin.org/post");
      if (status === 200) {
        message.success("Заказ успешно отправлен!");
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
