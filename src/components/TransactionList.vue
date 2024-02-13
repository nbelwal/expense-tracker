<script setup>
    import {defineProps} from 'vue'
    const props = defineProps({
        transactions: {
            type: Array,
            required: true
        }
    })
    const emit = defineEmits(['onDelete']) 
    const deleteTransaction = (id) => {
        emit('onDelete',id)
    }
</script>
<template>
     <h3>History</h3>
      <ul id="list" class="list" >
        <li :class="transaction.amount>0?'plus':'minus'" v-for="transaction in transactions" :key="transaction.id">
          {{ transaction.text }} <span>${{ transaction.amount }}</span>
          <button class="delete-btn" @click="()=>{deleteTransaction(transaction.id)}">x</button>
        </li> 
      </ul>
</template>