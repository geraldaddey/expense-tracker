<template>
    <div class="container">
        <Header />
        <Balance :total="+total"/>
        <IncomeExpense :income="+income" :expense="+expense"/>
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
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


import { ref, computed, onMounted } from 'vue'

onMounted( () => { 

    // check for transactions in local storage 
    const savedTransactions = JSON.parse( localStorage.getItem( 'transactions' ) );

    if ( savedTransactions ) {  
        transactions.value = savedTransactions;
    }
} )

const toast = useToast()

const transactions = ref(JSON.parse(localStorage.getItem('transactions')) || [])



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

    toast.success( 'Transaction Added' ); 
    
    saveTransactionsToLocalStorage
    
} 

//Delete Transaction 
const handleTransactionDeleted = ( id ) => { 
    transactions.value = transactions.value.filter( transaction => transaction.id !== id ) 
    
    saveTransactionsToLocalStorage 
    
    toast.success('Transaction Deleted');
}

const generateUniqueId = () => {
    return Math.floor( Math.random() * 100000000 );
}

const saveTransactionsToLocalStorage = () => {
    localStorage.setItem( 'transactions', JSON.stringify( transactions.value ) );
    console.log("saved to local storage",transactions.value)
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
