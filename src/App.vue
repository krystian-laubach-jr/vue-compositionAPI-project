<template>

  <MainHeader/>

  <div class="container">
    <UserBalance :balance="balance" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @submitTransaction="handleSubmitTransaction"/>
  </div>

</template>

<script setup>

  import MainHeader from './components/MainHeader.vue';
  import UserBalance from './components/UserBalance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  import { ref, computed, onMounted } from 'vue';

  import {useToast} from 'vue-toastification';
  const toast = useToast();

  import { sum } from 'lodash';

  import { v4 as uuid } from 'uuid';


  const transactions = ref([]);

  onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
      transactions.value = savedTransactions;
    }
  });

  const balance = computed(() => {
    return sum(transactions.value.map(transaction => transaction.amount));
  });

  const income = computed(() => {
    return sum(transactions.value.filter(transaction => transaction.amount > 0).map(transaction => transaction.amount));
  });

  const expenses = computed(() => {
    return Math.abs(sum(transactions.value.filter(transaction => transaction.amount < 0).map(transaction => transaction.amount)));
  });

  const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  };

  const handleSubmitTransaction = (transactionData) => {
    transactions.value.push({
      id: uuid(),
      text: transactionData.text,
      amount: transactionData.amount
    });

    saveTransactionsToLocalStorage();

    toast.success("Transaction added succesfully");
  };

  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
      (transaction) => transaction.id !== id
    );

    saveTransactionsToLocalStorage();

    toast.success('Transaction deleted succesfully');
  };

</script>
