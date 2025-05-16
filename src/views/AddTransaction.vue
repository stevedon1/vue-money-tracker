<template>
  <div class="add-transaction-container">
    <h2>Add New Transaction</h2>
    <form class="transaction-form" @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="amount">Amount</label>
        <input type="number" id="amount" v-model="amount" placeholder="Enter amount" />
      </div>

      <div class="form-group">
        <label>Type</label>
        <div class="radio-group">
          <label><input type="radio" v-model="type" name="type" value="income" /> Income</label>
          <label><input type="radio" v-model="type" name="type" value="expense" /> Expense</label>
        </div>
      </div>

      <div class="form-group">
        <label for="category">Category</label>
        <select id="category" v-model="category">
            <option value="Income & Salary">Income & Salary</option>    
            <option value="Housing">Housing/Rent</option>
            <option value="Food & Groceries">Food & Groceries</option>
            <option value="Utilities & Bills">Utilities & Bills</option>
            <option value="Transportation">Transportation</option>
            <option value="Savings & Investments">Savings & Investments</option>
            <option value="Health & Insurance">Health & Insurance</option>
            <option value="Education & Childcare">Education & Childcare</option>
            <option value="Entertainment & Lifestyle">Entertainment & Lifestyle</option>
            <option value="Miscellaneous">Miscellaneous(Others)</option>
        </select>
      </div>


      <div class="form-group">
        <label for="paymentMethod">Payment Method</label>
        <select id="paymentMethod" v-model="paymentMethod">
          <option>cash</option>
          <option>mpesa</option>
          <option>bank</option>
        </select>
      </div>

      <div class="form-group">
        <label for="description">Add a Description</label>
        <textarea id="description" v-model="description" rows="5" placeholder="eg.[Income]:I received rent/ salary/debt from my employer/dad/Juma   or [Expense]: I spent on house shopping. Or: I sent 500 to my son in school for upkeep"></textarea>
      </div>

      <div class="form-group">
        <label for="date">Date</label>
        <input type="date" id="date" v-model="date"/>
      </div>

      <button type="submit" class="submit-button">Add Transaction</button>
    </form>
  </div>
  <p v-if="error" style="color: red;">{{ error }}</p>
  <p v-if="success" style="color: green;">{{ success }}</p>

</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const amount = ref('')
const type = ref('')
const category = ref('')
const paymentMethod = ref('')
const description = ref('')
const date = ref('')
const error = ref('')
const success = ref('')

const handleSubmit = async () => {
  error.value = ''
  success.value = ''

  const token = localStorage.getItem('auth_token')

  if (!token) {
    error.value = 'No auth token found. Please login.'
    return
  }

  try {
    const res = await fetch('https://money-tracker-api-l3ud.onrender.com/api/transactions', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${token}`
      },
      body: JSON.stringify({
        amount: parseFloat(amount.value),
        type: type.value,
        category: category.value,
        paymentMethod: paymentMethod.value,
        description: description.value,
        date: new Date(date.value).toISOString()
      })
    })

    const data = await res.json()

    if (!res.ok) {
      throw new Error(data.message || 'Failed to add transaction')
    }

    success.value = 'Transaction added!'
    // Optional: redirect to home
    setTimeout(() => {
      router.push('/')
    }, 100)
  } catch (err) {
    error.value = err.message
  }
}
</script>


<style scoped>
.add-transaction-container {
  max-width: 500px;
  margin: 3rem auto;
  padding: 2rem;
  border: 1px solid #ccc;
  background-color: #fafafa;
  border-radius: 8px;
}

h2 {
  text-align: center;
  margin-bottom: 2rem;
}

.transaction-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  font-weight: 600;
  margin-bottom: 0.5rem;
}

input, select, textarea {
  padding: 0.5rem;
  font-size: 1rem;
  border: 1px solid #bbb;
  border-radius: 4px;
}

.radio-group {
  display: flex;
  gap: 1rem;
}

.submit-button {
  padding: 0.75rem;
  background-color: #1e40af;
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  /* transition: background 0.3s ease; */
}

.submit-button:hover {
  background-color: #1e3a8a;
}
</style>
