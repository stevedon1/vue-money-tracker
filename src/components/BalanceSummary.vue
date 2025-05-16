<template>
  <div class="summary-container">
    <div class="summary-box income">
      <h3>Total Income</h3>
      <p>$0</p> <!-- will be dynamic later -->
    </div>
    <div class="summary-box expense">
      <h3>Total Expenses</h3>
      <p>$0</p>
    </div>
    <div class="summary-box balance">
      <h3>Net Balance</h3>
      <p>$0</p>
    </div>
  </div>
</template>

<script setup>
// Logic will come later — this is the visual strike
</script>

<style scoped>
.summary-container {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
  padding: 1rem;
  margin: 2rem auto;
  max-width: 800px;
  flex-wrap: wrap;
}

.summary-box {
  flex: 1;
  min-width: 200px;
  background-color: #f4f4f4;
  border-radius: 8px;
  padding: 1rem;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.summary-box h3 {
  margin-bottom: 0.5rem;
}

.income {
  border-left: 5px solid #10b981; /* green */
}

.expense {
  border-left: 5px solid #ef4444; /* red */
}

.balance {
  border-left: 5px solid #3b82f6; /* blue */
}
</style>
