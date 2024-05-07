<template>
    <Header />
    <div class="container">
        <Balance :total="total" />
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <!-- pass transactions as props to TransactionList component -->
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { useToast } from 'vue-toastification';

// wraps variable to make it reactive to changes
import { ref, computed, onMounted } from 'vue';

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
});

// pass in transactions.value instead of only transactions
const total = computed(() => {
  // accumulator that starts at 0 and adds all transaction amounts
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

// get income
const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
});

// get expenses
const expenses = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
});

// handle submit
const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount,
    });

    saveTransactionsToLocalStorage();

    toast.success('Transaction added.');
};

// generate unique ID
const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
};

// delete transaction
const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
        (transaction) => transaction.id !== id
    );

    saveTransactionsToLocalStorage();

    toast.success('Transaction deleted.');
};

// save transactions to local storage
const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
};
</script>