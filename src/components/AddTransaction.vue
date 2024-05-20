<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Income / Expenses</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount">Amount<br/>
        (expenses - negative amount)</label>
      <input type="text" id="amount" placeholder="£ Enter amount..." v-model="amount" />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const text = ref('');
const amount = ref('');

const emit = defineEmits(['transactionSubmitted']);

const toast = useToast();

const onSubmit = () => {
  const textPattern = /^[a-zA-Z\s]+$/;
  const amountPattern = /^-?\d+(\.\d{1,2})?$/;

  if (!text.value || !amount.value) {
    toast.error("Both fields are required.");
    return;
  }

  if (!textPattern.test(text.value)) {
    toast.error("Text field should only contains letters");
    return;
  };

  if (!amountPattern.test(amount.value)) {
    toast.error("Amount field should be valid number, no spaces (negative for expenses)");
    return;
  }

  const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)
  }

  emit('transactionSubmitted', transactionData);

  text.value = '';
  amount.value = '';
};
</script>