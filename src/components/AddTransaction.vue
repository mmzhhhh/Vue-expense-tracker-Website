<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">
                Text
            </label>
            <input type="text" id="text" v-model="text" placeholder="Enter text..."/>
        </div>
        <div class="form-control">
            <label for="amount">
                Amount<br/>
                (ngative - expense, positive + income)
            </label>
            <input type="text" id="amount" v-model="amount" placeholder="Enter amount..."/>
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
import {ref} from 'vue'
import {useToast} from 'vue-toastification'

const text=ref('')
const amount=ref('');

const toast=useToast();

const emit=defineEmits(['transactionSubmitted']);


const onSubmit=()=>{
    if(!text.value||!amount.value){
        toast.error('Both fields must be filled.');
        return;
    }
    //console.log(text.value,amount.value);

    //数据
    const transactionData={
        text:text.value,
        amount:parseFloat(amount.value),
    }

    //发射
    emit('transactionSubmitted',transactionData);


    //置空
    text.value=''
    amount.value=''
}
</script>
