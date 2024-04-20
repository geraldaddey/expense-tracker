<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" v-bind:key="transaction.id":class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{transaction.text}} <span>{{ transaction.amount  }} GHS </span>
            <button class="delete-btn">x</button>
        </li>
    </ul>
</template>

<script setup>
import { defineProps } from 'vue'; 

const props = defineProps( {
    transactions: { 
        type: Array,
        required: true
    }
})
</script>

<!-- <script>
export default { 
    // Using the Options API below
    data () {  
        return {
            transactions: [
                { id: 1, text: 'Flower', amount: -20 },
                { id: 2, text: 'Salary', amount: 300 },
                { id: 3, text: 'Book', amount: -10 },
                { id: 4, text: 'Camera', amount: 150 }
            ]
        }
    }


    // Using the Composite API below 
    setup () {  
        const transactions = [
            { id: 1, text: 'Flower', amount: -20 },
            { id: 2, text: 'Salary', amount: 300 },
            { id: 3, text: 'Book', amount: -10 },
            { id: 4, text: 'Camera', amount: 150 }
        ]

        sortTransactions = () => {
            transactions.sort((a, b) => b.amount - a.amount)
        }

        return { transactions, sortTransactions }
    }
      
}

</script> -->
