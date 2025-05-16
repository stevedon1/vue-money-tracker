<template>
  <div class="transaction-list-container">
    <h2>Your Transactions</h2>

    <!-- Static mock data for now -->
    <div class="transaction-card">
      <div class="top-row">
        <span class="category">Rent</span>
        <span class="amount income">+ $300</span>
      </div>
      <div class="details">
        <span>Cash</span>
        <span>April 26, 2025</span>
      </div>
      <p class="description">pay may rent</p>
    </div>

    <!-- Add more dummy cards here if needed for layout testing -->
  </div>
</template>

<script setup>
// Logic and dynamic rendering will come after API connection
</script>

<style scoped>
.transaction-list-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 1rem;
}

.transaction-card {
  background-color: #fff;
  border: 1px solid #ddd;
  border-left: 5px solid #10b981; /* default green for income */
  border-radius: 6px;
  padding: 1rem;
  margin-bottom: 1rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
}

.top-row {
  display: flex;
  justify-content: space-between;
  font-weight: bold;
  font-size: 1.1rem;
}

.amount.income {
  color: #10b981;
}

.amount.expense {
  color: #ef4444;
}

.details {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
  color: #666;
  margin-top: 0.5rem;
}

.description {
  margin-top: 0.5rem;
  font-style: italic;
  color: #444;
}
</style>
