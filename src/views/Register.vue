<template>
  <div class="register-container">
    <p>
      Already have an account?
      <router-link to="/login">Login Here</router-link>
    </p>
    <h2>Register New Account</h2>

    <form @submit.prevent="handleRegister" class="register-form">
      <div class="form-group">
        <label for="name">Name</label>
        <input v-model="name" type="text" id="name" placeholder="Enter your name" />
      </div>

      <div class="form-group">
        <label for="email">Email</label>
        <input v-model="email" type="email" id="email" placeholder="Enter your email" />
      </div>

      <div class="form-group">
        <label for="password">Password</label>
        <input v-model="password" type="password" id="password" placeholder="Enter your password" />
      </div>

      <button type="submit" class="register-button">Register</button>
    </form>

    <p v-if="error" style="color: red; margin-top: 1rem;">{{ error }}</p>
    <p v-if="success" style="color: green; margin-top: 1rem;">{{ success }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const name = ref('')
const email = ref('')
const password = ref('')
const error = ref('')
const success = ref('')

const router = useRouter()

const handleRegister = async () => {
  error.value = ''
  success.value = ''

  try {
    const res = await fetch('https://money-tracker-api-l3ud.onrender.com/api/auth/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        name: name.value,
        email: email.value,
        password: password.value,
      }),
    })

    const data = await res.json()

    if (!res.ok) {
      throw new Error(data.message || 'Registration failed')
    }

    // ✅ Store the token in localStorage
    const token = data.token
    if (token) {
      localStorage.setItem('auth_token', token)
    } else {
      throw new Error('Token not found in response')
    }

    success.value = 'Registration successful! Redirecting...'

    // ✅ Redirect to home/dashboard
    setTimeout(() => {
      router.push('/')
    }, 1000)

  } catch (err) {
    error.value = err.message
  }
}
</script>



<style scoped>
.register-container {
  max-width: 400px;
  margin: 3rem auto;
  padding: 2rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  background: #f8f8f8;
}

h2 {
  text-align: center;
  margin-bottom: 1.5rem;
}

.register-form {
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

.form-group input {
  padding: 0.5rem;
  border: 1px solid #aaa;
  border-radius: 4px;
  font-size: 1rem;
}

.register-button {
  padding: 0.75rem;
  background-color: #1e40af;
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.register-button:hover {
  background-color: #1e3a8a;
}
</style>
