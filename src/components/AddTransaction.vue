<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.preven="onSubmit">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text..." v-model="text" />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="text" id="amount" placeholder="Enter amount..." v-model="amount" />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>


<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';


const text = ref( '' );
const amount = ref( '' );

const toast = useToast();


const onSubmit = () => {
    if ( !text.value || !amount.value ) {
        toast.error( 'Please add a text and amount' );
        return;
    }
    console.log( text.value, amount.value )
    console.log( 'submitted' )

    text.value = ''
    amount.value = ''
}

</script>
