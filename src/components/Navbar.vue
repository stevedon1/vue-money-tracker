<template>
  <nav class="navbar">
    <div class="logo">FinanceTracker App</div>
    <router-link to="/add-transaction" class="add-btn">
      Add Transaction
    </router-link>
  </nav>
</template>

<script setup>
// No JS logic needed for this simple navbar
</script>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background-color: #1f2937; /* Dark slate */
  color: white;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.logo {
  font-size: 1.5rem;
  font-weight: bold;
  letter-spacing: 1px;
}

.add-btn {
  padding: 0.5rem 1rem;
  background-color: #10b981; /* Emerald green */
  color: white;
  border: none;
  border-radius: 4px;
  text-decoration: none;
  font-weight: 600;
  transition: background-color 0.3s ease;
}

.add-btn:hover {
  background-color: #059669; /* Darker green */
}

@media (max-width: 600px) {
  .navbar {
    flex-direction: column;
    align-items: flex-start;
  }

  .add-btn {
    margin-top: 0.5rem;
    align-self: stretch;
    text-align: center;
  }
}
</style>
