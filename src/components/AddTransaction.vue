<template>

  <h3>Add new transaction</h3>

  <form id="form" @submit.prevent="onSubmit">

    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" autocomplete="off" placeholder="Enter text..." v-model="text"/>
    </div>

    <div class="form-control">
      <label for="amount">
      Amount <br/>
      (negative - expense, positive - income)
      </label>

      <input type="number" id="amount" step="0.01" placeholder="Enter amount..." v-model="amount"/>
      
    </div>

    <button class="btn">Add transaction</button>

  </form>

</template>

<script setup>
  import { ref, defineEmits } from 'vue';

  import {useToast} from 'vue-toastification';
  const toast = useToast();

  const emit = defineEmits(['submitTransaction']);

  const text = ref('');
  const amount = ref('');

  const onSubmit = () => {

    if (!text.value || !amount.value) {
      toast.error("Input cannot be empty!");
      return;
    }

    const transactionData = {
      text: text.value,
      amount: parseFloat(amount.value)
    }

    emit('submitTransaction', transactionData);

    text.value = '';
    amount.value = '';

  }

</script>