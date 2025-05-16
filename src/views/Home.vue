<template>
    <Navbar/>
    <BalanceSummary/>
    <TransactionList/>
</template>

<script setup>
import BalanceSummary from '@/components/BalanceSummary.vue';
import Navbar from '@/components/Navbar.vue';
import TransactionList from '@/components/TransactionList.vue';

const token = localStorage.getItem('auth_token')
</script>