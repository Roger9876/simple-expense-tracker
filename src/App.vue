<template>
  <HeaderComponent />
  <div class="container">
    <BalanceComponent :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList @delete-transaction="handleTransactionDeleted" :transactions="transactions" />
    <AddTransaction @add-transaction="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import HeaderComponent from './components/HeaderComponent.vue';
import BalanceComponent from './components/BalanceComponent.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { useToast } from 'vue-toastification';
import { ref, computed, onMounted } from 'vue';

const toast = useToast();

const transactions = ref([]);

// Check if there are any transactions in local storage
onMounted(() => {
  const storedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (storedTransactions) {
    transactions.value = storedTransactions;
  }
});

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0).toFixed(2);
});

// Get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0).toFixed(2);
});

// Handle transaction submitted
const handleTransactionSubmitted = (newTransaction) => {
  transactions.value.push({
    id: generareUniqueId(),
    text: newTransaction.text,
    amount: newTransaction.amount,
  });
  saveTransactionsToLocalStorage();
  toast.success('Transaction added successfully!');
};

// Generate unique id
const generareUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

// Handle transaction deleted
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
  saveTransactionsToLocalStorage();
  toast.success('Transaction deleted successfully!');
};

// Save transactions to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};
</script>
