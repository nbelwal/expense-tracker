<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue'
import {computed,onMounted,ref} from 'vue'
import {useToast} from 'vue-toastification'
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
const transactions = ref([{id : 1, text : "some income", amount : 50},
{id : 2, text : "some expense", amount : -20}
])
const toast = useToast()
onMounted(()=>{
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if(savedTransactions)
    transactions.value=savedTransactions
})
//total
const balance = computed(()=>{
  return transactions.value.reduce((acc,transaction)=>{
    return acc+transaction.amount
  },0)
})
  //income
const income = computed(()=>{
  return transactions.value
  .filter(transaction=>transaction.amount>0)
  .reduce((acc,transaction)=>acc+transaction.amount,0)
  .toFixed(2)
})
//expense
const expense = computed(()=>{
  return transactions.value
  .filter(transaction=>transaction.amount<0)
  .reduce((acc,transaction)=>acc+transaction.amount,0)
  .toFixed(2)
})
//handleTransactionSubmitted
const handleTransactionSubmitted = (transactionData)=>{
  // console.log(transactionData)
  transactions.value.push(transactionData)
  saveTransactionToBrowser()
  toast.success('transaction added')
}
const handleDelete = (id)=>{
  transactions.value=transactions.value.filter((transaction)=>transaction.id!=id)
  saveTransactionToBrowser()
  toast.success('transaction deleted')
}
const saveTransactionToBrowser =()=>{
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
<Header/>
<div class="container">
  <Balance :balance = "+balance"/>
  <IncomeExpense :income="+income" :expense="+expense"/>
  <TransactionList :transactions="transactions" @onDelete="handleDelete" />
  <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
</div>
</template>

<style scoped>
</style>