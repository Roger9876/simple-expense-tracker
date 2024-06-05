<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="text" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">
        Amount <br />
        (expense = negative, income = positive)
      </label>
      <input type="number" id="amount" v-model="amount" placeholder="Enter amount...">
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';

const text = ref('');
const amount = ref('');

const emit = defineEmits(['add-transaction']);

const toast = useToast();

// Handle form submit
const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Both fields are required!');
    return;
  }

  const newTransaction = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  emit('add-transaction', newTransaction);

  text.value = '';
  amount.value = '';
};
</script>
