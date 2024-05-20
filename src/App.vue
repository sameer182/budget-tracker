<template>
  <Header />
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([
  { id: 1, text: 'Flower', amount: -19.99 },
  { id: 2, text: 'Salary', amount: 2000 },
  { id: 3, text: 'Book', amount: -10.50 },
  { id: 4, text: 'Camera', amount: 20 },
]);

// Get total amount
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
});

// Add transaction
const handleTransactionSubmitted = (transactionData) => {
   transactions.value.push({
    id: generateId(),
    text: transactionData.text,
    amount: transactionData.amount
   });
    toast.success('Added successfully');

};

// Generate unique Id
const generateId = () => {
  return Math.floor(Math.random() * 10000);
}

</script>