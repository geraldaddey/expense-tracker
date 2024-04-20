<template>
    <div class="container">
        <Header />
        <div class="content">
            <div class="balance">
                <h2 class="balance-title">Your Balance</h2>
                <h3 class="balance-amount">{{ total }}</h3>
            </div>
            <div class="income-expense">
                <div class="income">
                    <h4 class="income-title">Income</h4>
                    <p class="income-amount">GHS {{ income }}</p>
                </div>
                <div class="expense">
                    <h4 class="expense-title">Expense</h4>
                    <p class="expense-amount"> GHS {{ expense }}</p>
                </div> 
            </div>
            <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
            <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
        </div>
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions')) || []
});

const toast = useToast();

const transactions = ref(JSON.parse(localStorage.getItem('transactions')) || []);

const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

const income = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0)
        .toFixed(2);
});

const expense = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0)
        .toFixed(2);
});

const handleTransactionSubmitted = (transactionDetails) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionDetails.text,
        amount: transactionDetails.amount,
    });

    toast.success('Transaction Added');

    saveTransactionsToLocalStorage();
};

const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
        (transaction) => transaction.id !== id
    );

    saveTransactionsToLocalStorage();

    toast.success('Transaction Deleted');
};

const generateUniqueId = () => {
    return Math.floor(Math.random() * 100000000);
};

const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
    console.log('saved to local storage', transactions.value);
};
</script>

<style scoped>
.container {
    max-width: 400px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f5f5f5;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.content {
    margin-top: 20px;
}

.balance {
    text-align: center;
    margin-bottom: 20px;
}

.balance-title {
    font-size: 24px;
    color: #333;
}

.balance-amount {
    font-size: 36px;
    color: #333;
}

.income-expense {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
}

.income,
.expense {
    flex: 1;
    text-align: center;
}

.income-title,
.expense-title {
    font-size: 16px;
    color: #333;
}

.income-amount,
.expense-amount {
    font-size: 24px;
    color: #333;
}

.transaction-list {
    margin-bottom: 20px;
}

.add-transaction {
    text-align: center;
}

.add-transaction input[type='text'],
.add-transaction input[type='number'] {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.add-transaction button {
    width: 100%;
    padding: 8px;
    background-color: #333;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.add-transaction button:hover {
    background-color: #555;
}
</style>
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
