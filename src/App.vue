<template>

  <MainHeader/>

  <div class="container">
    <UserBalance :balance="balance" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList :transactions="transactions" />
    <AddTransaction @submitTransaction="handleSubmitTransaction"/>
  </div>

</template>

<script setup>

  import MainHeader from './components/MainHeader.vue';
  import UserBalance from './components/UserBalance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { ref, computed } from 'vue';

  import {useToast} from 'vue-toastification';
  const toast = useToast();

  import { sum } from 'lodash';

  import { v4 as uuid } from 'uuid';

  const transactions = ref([
    { id: 1, text: 'Flower', amount: -19.99 },
    { id: 2, text: 'Salary', amount: 299.97 },
    { id: 3, text: 'Book', amount: -10 },
    { id: 4, text: 'Camera', amount: 150 }
  ]);

  const balance = computed(() => {
    return sum(transactions.value.map(transaction => transaction.amount));
  });

  const income = computed(() => {
    return sum(transactions.value.filter(transaction => transaction.amount > 0).map(transaction => transaction.amount));
  });

  const expenses = computed(() => {
    return Math.abs(sum(transactions.value.filter(transaction => transaction.amount < 0).map(transaction => transaction.amount)));
  });

  const handleSubmitTransaction = (transactionData) => {
    transactions.value.push({
      id: uuid(),
      text: transactionData.text,
      amount: transactionData.amount
    });

    toast.success("Transaction added succesfully");
};
</script>
