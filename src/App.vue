
<template>
    <Header />
    <div class="container">
        <Balance :total="total" />
        <IncomeExpenses />
        <!-- pass transactions as props to TransactionList component -->
        <TransactionList :transactions="transactions" />
        <AddTransaction />
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

// wraps variable to make it reactive to changes
import { ref, computed } from 'vue';

const transactions = ref([
    { id: 1, text: 'Flower', amount: -19.11 },
    { id: 2, text: 'Salary', amount: 229.97 },
    { id: 3, text: 'Book', amount: -10 },
    { id: 4, text: 'Camera', amount: 150 }
])

// pass in transactions.value instead of only transactions
const total = computed(()=>{
    // accumulator that starts at 0 and adds all transaction amounts
    return transactions.value.reduce((acc, transaction)=>{
        return acc + transaction.amount;
    }, 0)
});

// get income

// get expenses
</script>