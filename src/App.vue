<template>
  <MainHeader/>
  <div class="container">
    <UserBalance :balance="balance"/>
    <IncomeExpenses :income="income" :expenses="expenses"/>
    <TransactionList :transactions="transactions"/>
    <AddTransaction/>
  </div>
</template>

<script setup>
import MainHeader from './components/MainHeader.vue';
import UserBalance from './components/UserBalance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed } from 'vue';

import { sum } from 'lodash';

const balance = computed(() => {
  return sum(transactions.value.map(transaction => transaction.amount));
});

const transactions = ref([
        { id: 1, text: 'Flower', amount: -19.99 },
        { id: 2, text: 'Salary', amount: 299.97 },
        { id: 3, text: 'Book', amount: -10 },
        { id: 4, text: 'Camera', amount: 150 }
    ]);

const income = computed(() => {
  return sum(transactions.value.filter(transaction => transaction.amount > 0).map(transaction => transaction.amount));
});

const expenses = computed(() => {
  return Math.abs(sum(transactions.value.filter(transaction => transaction.amount < 0).map(transaction => transaction.amount)));
});
</script>
