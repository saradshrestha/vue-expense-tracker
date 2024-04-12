<template>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="onSubmit">
        <div class="form-control">
            <label for="text">Title</label>
            <input type="text" id="text" v-modal="title" placeholder="Enter text..." />
        </div>
        <div class="form-control">
            <label for="amount">Amount <br />
                (negative - expense, positive - income)</label>
            <input type="number" id="amount" placeholder="Enter amount..." />
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
    import {ref} from 'vue';
    import {useToast} from 'vue-toastification';

    const toast =useToast();
    const emit = defineEmits(['transactionSubmitted']);
    const amount = ref('');
    const title = ref('');

    const onSubmit= () => {
        if(!title.value || !amount.value){
            toast.error('Both Fields are required.')
            return;
        }
        const transactionData = {
            title: title.value,
            amount: parseFloat(amount.value)
        }
        emit('transactionSubmitted',transactionData);

    };
</script>