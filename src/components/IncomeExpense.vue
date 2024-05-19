<template>
    <div class="inc-exp-container">
        <div>
            <h4>Income</h4>
            <p id="money-plus" class="money plus">+${{ formattedIncome }}</p>
        </div>
        <div>
            <h4>Expense</h4>
            <p id="money-minus" class="money minus">${{ formattedExpenses }}</p>
        </div>
    </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    income: {
        type: Number,
        required: true
    },
    expenses: {
        type: Number,
        required: true
    }
});

const formattedIncome = computed(() => props.income.toFixed(2));
const formattedExpenses = computed(() => props.expenses.toFixed(2));
</script>