<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    transactions: {
        type: Array,
        isrequired: true
    }
})

const emit = defineEmits(["transaction-deleted"])

const deleteTransaction = (id) => {
    emit('transaction-deleted', id)

}



</script>

<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{ transaction.name }} <span>${{ transaction.amount }}</span><button class="delete-btn"
                @click="deleteTransaction(transaction.id)">x</button>
        </li>

    </ul>
</template>

