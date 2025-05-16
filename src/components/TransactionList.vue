<template>
  <div class="transaction-list-container">
    <h2>Your Transactions</h2>

    <div
      v-for="tx in transactions"
      :key="tx._id"
      class="transaction-card"
      :style="{ borderLeftColor: tx.type === 'income' ? '#10b981' : '#ef4444' }"
    >
      <div class="top-row">
        <span class="category">{{ tx.category }}</span>
        <span :class="['amount', tx.type]">
          {{ tx.type === 'income' ? '+' : '-' }} ${{ tx.amount }}
        </span>
      </div>

      <div class="details">
        <span>{{ tx.paymentMethod }}</span>
        <span>{{ formatDate(tx.date) }}</span>
      </div>

      <p class="description">{{ tx.description }}</p>
    </div>
  </div>
</template>


<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const transactions = ref([])
const router = useRouter()

// format date
function formatDate(dateStr) {
  const date = new Date(dateStr)
  return date.toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  })
}


onMounted(async () => {
  const token = localStorage.getItem('auth_token')

  if (!token) {
    router.push('/register')
    return
  }

  try {
    const res = await fetch('https://money-tracker-api-l3ud.onrender.com/api/transactions', {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    })

    const data = await res.json()

    if (!res.ok) {
      throw new Error(data.message || 'Failed to fetch transactions')
    }

    transactions.value = data
    console.log('Fetched transactions:', data)
  } catch (err) {
    console.error('Error fetching transactions:', err)
    // Optional: clear token if invalid
    localStorage.removeItem('token')
    router.push('/register')
  }
})
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
