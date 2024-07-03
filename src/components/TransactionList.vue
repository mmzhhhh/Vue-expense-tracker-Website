<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount<0?'minus':'plus'">
            {{ transaction.text }}
            <span>
                ${{ transaction.amount }}
            </span>
            <button class="delete-btn" @click="deleteTransaction(transaction.id)">x</button>
        </li>
    </ul>
</template>

<script setup>

const props=defineProps({
    transactions:{
        type:Array,
        //必须提供这个属性
        required:true
    }
})

//发射要删除的id
const emit=defineEmits(['transactionDeleted']);

//获取要删除的id
const deleteTransaction=(id)=>{
    emit('transactionDeleted',id)
}
</script>
