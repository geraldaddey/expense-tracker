<template>
    <div class="container">
        <Header />
        <Balance />
        <IncomeExpense />
        <TransactionList :transactions="transactions"/>
        <AddTransaction />
    </div>
</template>


<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue'; 


import { ref } from 'vue'

const transactions = ref([
    { id: 1, text: 'Flower', amount: -20 },
    { id: 2, text: 'Salary', amount: 300 },
    { id: 3, text: 'Book', amount: -10 },
    { id: 4, text: 'Camera', amount: 150 }
] )
</script>


<!-- <script>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

export default {
    components: {
        Header,
        Balance,
        IncomeExpense,
        TransactionList,
        AddTransaction
    }
}
</script> -->
