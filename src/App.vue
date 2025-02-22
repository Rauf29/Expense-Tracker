<script setup>
import { onMounted, ref, watch } from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import TransactionList from './components/TransactionList.vue';

const transactions = ref([]);

onMounted(function () {
  const savedTransactions = localStorage.getItem('transactions');
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});

watch(transactions, function () {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}, { deep: true });

function addTransaction(transaction) {
  transactions.value.push(transaction);
}

function deleteTransaction(index) {
  transactions.value.splice(index, 1);
}

function filterTransactions(filterType) {
  if (filterType === 'all') return transactions.value;
  return transactions.value.filter(function (t) {
    return t.type === filterType.toUpperCase();
  });
}
</script>

<template>
  <div class="container mt-5">
    <AddTransaction @add-transaction="addTransaction" />
    <TransactionList :transactions="transactions" @delete-transaction="deleteTransaction"
      @filter-transactions="filterTransactions" />
  </div>
</template>

<style scoped>
body {
  background-color: #333;
  color: #fff;
}
</style>