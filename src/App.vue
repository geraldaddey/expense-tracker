<template>
    <div class="container">
        <Header />
        <Balance :total="+total"/>
        <IncomeExpense :income="+income" :expense="+expense"/>
        <TransactionList :transactions="transactions" />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
</template>


<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { useToast } from 'vue-toastification';


import { ref, computed } from 'vue'

const toast = useToast();

const transactions = ref( [
    { id: 1, text: 'Flower', amount: -20 },
    { id: 2, text: 'Salary', amount: 300 },
    { id: 3, text: 'Book', amount: -10 },
    { id: 4, text: 'Camera', amount: 1900 },
    { id: 5, text: 'Box', amount: -44 }
] )

// console.log(transactions.value)


// Total Balance
const total = computed( () => {
    return transactions.value.reduce( ( acc, transaction ) => {
        return acc + transaction.amount;
    }, 0 );
} )

//Income 
const income = computed( () => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce( ( acc, transaction ) => {
        return acc + transaction.amount;
    }, 0 )
    .toFixed( 2 );

} )

//Expense 
const expense = computed( () => {
    return transactions.value
        .filter( (transaction) => transaction.amount < 0 )
        .reduce( ( acc, transaction ) => {
            return acc + transaction.amount;
        }, 0 )
        .toFixed(2);
} )


//Add Transaction 
const handleTransactionSubmitted = ( transactionDetails ) => { 
    transactions.value.push( { 
        id: generateUniqueId(),
        text: transactionDetails.text,
        amount: transactionDetails.amount
    } ) 
    
    toast.success('Transaction Added');
} 


const generateUniqueId = () => {
    return Math.floor( Math.random() * 100000000 );
}

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
