<template>
  <Header/>
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :getIncome="(+getIncome).toFixed(2)" :getExpenses="(+getExpenses).toFixed(2)"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSumbitted" />
  </div>
</template>



<script setup>
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import TransactionList from './components/TransactionList.vue';
    import AddTransaction from './components/AddTransaction.vue';
    import {useToast} from 'vue-toastification';

    import { ref,computed } from 'vue';

    const toast = useToast();

    const transactions = ref( [
      { id:1, text:'Flower',amount:20  },
      { id:2, text:'Salary',amount:20  },
      { id:3, text:'Book',amount:20  },
      { id:4, text:'Camera',amount:-20  },
    ]);

    const total = computed(() => {
      return transactions.value.reduce((acc,transaction) => {
        return acc + transaction.amount;
      },0);
    });

    const getIncome = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount > 0)
      .reduce((acc,transaction) => {
        return acc + transaction.amount;
      }, 0);
    });

    const getExpenses = computed(() => {
      return transactions.value
      .filter((transaction) => transaction.amount < 0)
      .reduce((acc,transaction) => {
        return acc + transaction.amount;
      }, 0);
    });

    //Add Transaction
    const handleTransactionSumbitted = (transactionData) =>{
      console.log(transactionData);
      transactions.value.push[{
        id:generateUnqiueId(),
        text:transactionData.title,
        amount:transactionData.amount,
      }]

      toast.success('Transaction added.')
    }

    const generateUnqiueId = () =>{
      return Math.floor(Math.random());
    }

    const handleTransactionDeleted = () => {
        transactions.value = transactions.value.filter(
          (transaction) => transaction.id !== id
        );
        toast.success('Transaction deleted.')
    }




</script>