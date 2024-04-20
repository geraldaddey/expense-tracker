<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" v-bind:key="transaction.id":class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{transaction.text}} <span>{{ transaction.amount  }} GHS </span>
            <button class="delete-btn">x</button>
        </li>


        <!-- <li class="minus">
            Cash <span>GHS 400 </span>
            <button class="delete-btn">x</button>
        </li>
        <li class="plus">
            Credit <span>GHS 200 </span>
            <button class="delete-btn">x</button>
        </li> -->
    </ul>
</template>


<script>
export default { 
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
    
}

</script>
