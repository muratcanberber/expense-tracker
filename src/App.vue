<script setup>
import Headers from "./components/Headers.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from 'vue-toastification';

const toast = useToast();


import { ref, computed, onMounted } from 'vue'

//OnMounted

onMounted(() => {
    const savedTransaction = JSON.parse(localStorage.getItem('transactions'))

    if (savedTransaction) {
        transactions.value = savedTransaction;
    }
})


const transactions = ref([])

// Get total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

//Get income
const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
});

//Get Expense
const expenses = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0)
        .toFixed(2);
});


//Handle Transaction Submitted

const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        name: transactionData.text,
        amount: transactionData.amount
    })
    saveTransactionsToLocalStorage();
    toast.success("Transaction Added")
}
//Generate Transaction 
const generateUniqueId = () => {
    return Math.floor(Math.random() * 10000000)
}


//Delete Transaction
const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    toast.success("Transaction deleted")
    saveTransactionsToLocalStorage();

}


//Save to LocalStorage


const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script> 
<template>
    <Headers />
    <div class="container">
        <Balance :total="total" />
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <TransactionList :transactions="transactions" @transaction-deleted="handleTransactionDeleted" />
        <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
    </div>
</template>
