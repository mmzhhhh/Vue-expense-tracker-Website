<template>
  <Header/>
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpense :income="+income" :expense="+expense"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>

<script setup>
import AddTransaction from './components/AddTransaction.vue'
import Balance from './components/Balance.vue'
import Header from './components/Header.vue';
import IncomeExpense from './components/IncomeExpense.vue'
import TransactionList from './components/TransactionList.vue'

import {ref,computed,onMounted} from 'vue'

import {useToast} from 'vue-toastification'

const toast=useToast();

const transactions=ref([])

onMounted(()=>{
  const savedTransactions=JSON.parse(localStorage.getItem('transactions'));

  if(savedTransactions){
    transactions.value=savedTransactions;
  }
})
//余额
const total=computed(()=>{
  return transactions.value.reduce((acc,transaction)=>{
    return acc+transaction.amount
  },0)
});

//收入
const income=computed(()=>{
  return transactions.value
  .filter((transaction)=>transaction.amount>0)
  .reduce((acc,transaction)=>acc+transaction.amount,0)
  .toFixed(2);
})

//支出
const expense=computed(()=>{
  return transactions.value
  .filter((transaction)=>transaction.amount<0)
  .reduce((acc,transaction)=>acc+transaction.amount,0)
  .toFixed(2);
})

//处理添加发射
const handleTransactionSubmitted=(transactionData)=>{
  transactions.value.push({
    id:generateUniqueId(),
    text:transactionData.text,
    amount:transactionData.amount,
  })

  saveTransactionsToLocalStorage();

  toast.success('Transaction added.');
}

//生成ID方法
const generateUniqueId=()=>{
  return Math.floor(Math.random()*1000000);
}

//删除方法
const handleTransactionDeleted=(id)=>{
  transactions.value=transactions.value.filter((transaction)=>transaction.id!==id);
  
  saveTransactionsToLocalStorage();

  toast.success('Transaction delete.');

}

//保存数据
const saveTransactionsToLocalStorage=()=>{
  localStorage.setItem('transactions',JSON.stringify(transactions.value));
}

</script>
