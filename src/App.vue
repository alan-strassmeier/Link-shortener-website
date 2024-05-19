<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { useToast } from 'vue-toastification';

import { computed, ref, onMounted } from 'vue';

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = localStorage.getItem('transactions');
  try {
    const parsedTransactions = JSON.parse(savedTransactions);
    if (Array.isArray(parsedTransactions)) {
      transactions.value = parsedTransactions;
    }
  } catch (error) {
    console.error('Failed to parse transactions from localStorage:', error);
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
});

const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
});

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  });
  saveTransactionsToLocalStorage();

  toast.success('Transaction added');
};

const generateUniqueId = () => {
  return Math.floor(Math.random() * 10000);
};

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) =>
    transaction.id !== id);
  saveTransactionsToLocalStorage();

  toast.success('Transaction deleted');
};

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="income" :expenses="expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
  </div>
</template>