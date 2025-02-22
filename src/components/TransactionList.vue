<script setup>
import { defineEmits, defineProps, ref } from 'vue';

const props = defineProps(['transactions']);
const emit = defineEmits(['delete-transaction']);
const filterType = ref('all');

function handleDelete(index) {
    emit('delete-transaction', index);
}

function applyFilter(type) {
    filterType.value = type;
}

const filteredTransactions = () => {
    if (filterType.value === 'all') return props.transactions;
    return props.transactions.filter(function (t) {
        return t.type === filterType.value.toUpperCase();
    });
};
</script>

<template>
    <div class="text-center mb-3">
        <label><input type="radio" v-model="filterType" value="all"> All</label>
        <label><input type="radio" v-model="filterType" value="Income"> Income</label>
        <label><input type="radio" v-model="filterType" value="Expense"> Expense</label>
    </div>

    <table class="table table-dark table-striped">
        <thead>
            <tr>
                <th>Title</th>
                <th>Amount</th>
                <th>Type</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(transaction, index) in filteredTransactions()" :key="index">
                <td>{{ transaction.title }}</td>
                <td
                    :class="{ 'text-success': transaction.type === 'INCOME', 'text-danger': transaction.type === 'EXPENSE', 'fw-bold': transaction.amount >= 500 }">
                    ${{ transaction.amount }}
                </td>
                <td>{{ transaction.type }}</td>
                <td>
                    <button @click="handleDelete(index)" class="btn btn-danger btn-sm">Delete</button>
                </td>
            </tr>
            <tr v-if="filteredTransactions().length === 0">
                <td colspan="4" class="text-center">No transactions recorded yet.</td>
            </tr>
        </tbody>
    </table>
</template>

<style scoped>
.table {
    max-width: 800px;
    margin: 0 auto;
}

label {
    margin-right: 10px;
    cursor: pointer;
}
</style>