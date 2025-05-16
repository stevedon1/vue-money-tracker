<template>
  <div class="summary-container">
    <div class="summary-box income">
      <h3>Total Income</h3>
      <p>{{ formatMoney(totalIncome) }}</p>
    </div>
    <div class="summary-box expense">
      <h3>Total Expenses</h3>
      <p>{{ formatMoney(totalExpense) }}</p>
    </div>
    <div class="summary-box balance">
      <h3>Net Balance</h3>
      <p>{{ formatMoney(netBalance) }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const transactions = ref([])

// Fetch transactions on mount
onMounted(async () => {
  const token = localStorage.getItem('auth_token') // ensure token name matches
  if (!token) return

  try {
    const res = await fetch('https://money-tracker-api-l3ud.onrender.com/api/transactions', {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })
    const data = await res.json()
    if (res.ok) {
      transactions.value = data
    }
  } catch (err) {
    console.error('Error fetching transactions:', err)
  }
})

// Computed totals
const totalIncome = computed(() =>
  transactions.value
    .filter((tx) => tx.type === 'income')
    .reduce((sum, tx) => sum + tx.amount, 0)
)

const totalExpense = computed(() =>
  transactions.value
    .filter((tx) => tx.type === 'expense')
    .reduce((sum, tx) => sum + tx.amount, 0)
)

const netBalance = computed(() => totalIncome.value - totalExpense.value)

// Currency formatter
const formatMoney = (amount) =>
  new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    minimumFractionDigits: 0,
  }).format(amount)
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
