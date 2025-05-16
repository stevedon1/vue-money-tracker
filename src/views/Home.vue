<template>
    <Navbar/>
    <h2>The HomePage</h2>
    <p>My token: {{ token }}</p>
</template>

<script setup>
import Navbar from '@/components/Navbar.vue';

const token = localStorage.getItem('auth_token')
</script>