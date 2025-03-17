<template>
  <MainHeader/>
  <div class="container">
    <UserBalance/>
    <IncomeExpenses/>
    <TransactionList/>
    <AddTransaction/>
  </div>
</template>

<script setup>
import MainHeader from './components/MainHeader.vue';
import UserBalance from './components/UserBalance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
</script>
