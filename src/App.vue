<template>
  <HeaderCom />
  <div class="container">
    <BalanceCom :total="total" />
    <IncomeExpenses :income="income" :expense="expense" />
    <TransactionList :transactions="transactions" @deleteTransaction="hanldeDelete" />
    <AddTransaction @submit-transaction="handleSubmit" />
  </div>
</template>

<script setup>
import HeaderCom from './components/HeaderCom.vue'
import BalanceCom from './components/BalanceCom.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { computed, onMounted, ref } from 'vue'
import { useToast } from 'vue-toastification'

const transactions = ref([])
const toast = useToast()

// mounted
onMounted(() => {
  console.log('mounted')
  const savedTransaction = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransaction) {
    transactions.value = savedTransaction
  }
})
// set
const savedTransaction = (data) => {
  return localStorage.setItem('transactions', JSON.stringify(data))
}
// get total by transactions
const total = computed(() => {
  return transactions.value.reduce((x, y) => {
    return x + y.amount
  }, 0)
})
// get income
const income = computed(() => {
  return transactions.value.reduce((x, y) => {
    return y.amount > 0 ? x + y.amount : x
  }, 0)
})
// get expense
const expense = computed(() => {
  return transactions.value.reduce((x, y) => {
    return y.amount < 0 ? x + y.amount : x
  }, 0)
})

const handleSubmit = (obj) => {
  transactions.value.push(obj)
  savedTransaction(transactions.value)
}
const hanldeDelete = (id) => {
  transactions.value = transactions.value.filter((x) => {
    return x.id !== id
  })
  savedTransaction(transactions.value)
  toast.success('Delete')
}
</script>
