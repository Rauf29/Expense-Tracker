<script setup>
import { defineEmits, ref } from 'vue';

const title = ref('');
const amount = ref(0);
const type = ref('Expense');
const errorMessage = ref('');

const emit = defineEmits(['add-transaction']);

function handleSubmit() {
    if (title.value.trim() === '' && amount.value <= 0) {
        errorMessage.value = 'Title and amount must be valid.';
        return;
    } else if (title.value.trim() === '') {
        errorMessage.value = 'Title must be valid.';
        return;
    } else if (amount.value <= 0) {
        errorMessage.value = 'Amount must be valid.';
        return;
    }
    emit('add-transaction', {
        title: title.value,
        amount: Number(amount.value),
        type: type.value.toUpperCase()
    });
    title.value = '';
    amount.value = 0;
    type.value = 'Expense';
    errorMessage.value = '';
}
</script>

<template>
    <form @submit.prevent="handleSubmit" class="mb-4">
        <div class="input-group mb-2">
            <input v-model="title" type="text" placeholder="Title" class="form-control">
            <input v-model="amount" type="number" placeholder="Amount" class="form-control">
            <select v-model="type" class="form-select">
                <option>Income</option>
                <option>Expense</option>
            </select>
            <button type="submit" class="btn btn-primary">Add</button>
        </div>
        <div v-if="errorMessage" class="text-danger">{{ errorMessage }}</div>
    </form>
</template>

<style scoped>
form {
    max-width: 600px;
    margin: 0 auto;
}
</style>
