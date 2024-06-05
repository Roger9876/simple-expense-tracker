<template>
  <h3>History</h3>
  <p v-if="transactions.length == 0">Nothing to show. Add something!</p>
  <ul id="list" class="list">
    <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from 'vue';

const emit = defineEmits(['delete-transaction']);

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const deleteTransaction = (id) => {
  emit('delete-transaction', id);
};
</script>
