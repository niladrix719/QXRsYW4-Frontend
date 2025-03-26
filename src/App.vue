<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'

const isLogin = ref(true)
const username = ref('')
const password = ref('')
const error = ref('')
const success = ref('')
const isAuthenticated = ref(false)
const currentUser = ref('')

// Check localStorage on component mount
onMounted(() => {
  const savedUser = localStorage.getItem('currentUser')
  const savedAuth = localStorage.getItem('isAuthenticated')
  if (savedUser && savedAuth) {
    currentUser.value = savedUser
    isAuthenticated.value = true
  }
})

const api = axios.create({
  baseURL: 'http://localhost:8080/api/auth',
  headers: {
    'Content-Type': 'application/json'
  }
})

const handleSubmit = async () => {
  error.value = ''
  success.value = ''
  
  try {
    const endpoint = isLogin.value ? '/login' : '/register'
    const response = await api.post(endpoint, {
      username: username.value,
      password: password.value
    })

    success.value = response.data.message
    if (isLogin.value) {
      isAuthenticated.value = true
      currentUser.value = username.value
      // Save to localStorage
      localStorage.setItem('currentUser', username.value)
      localStorage.setItem('isAuthenticated', 'true')
    }
    username.value = ''
    password.value = ''
    
    if (!isLogin.value) {
      isLogin.value = true
    }
  } catch (err: any) {
    if (err.response) {
      error.value = err.response.data.error
    } else {
      error.value = 'An error occurred. Please try again.'
    }
  }
}

const handleLogout = () => {
  isAuthenticated.value = false
  currentUser.value = ''
  success.value = 'Logged out successfully'
  // Clear localStorage
  localStorage.removeItem('currentUser')
  localStorage.removeItem('isAuthenticated')
}
</script>

<template>
  <div v-if="!isAuthenticated" class="auth-container">
    <h1>{{ isLogin ? 'Login' : 'Sign Up' }}</h1>
    <form @submit.prevent="handleSubmit" class="auth-form">
      <div v-if="error" class="error-message">{{ error }}</div>
      <div v-if="success" class="success-message">{{ success }}</div>
      <div class="form-group">
        <input v-model="username" type="text" placeholder="Username" required />
      </div>
      <div class="form-group">
        <input v-model="password" type="password" placeholder="Password" required />
      </div>
      <div class="btn-container">
        <button type="submit">{{ isLogin ? 'Login' : 'Sign Up' }}</button>
      </div>
      <p class="toggle-text">
        {{ isLogin ? "Don't have an account?" : "Already have an account?" }}
        <a href="#" @click.prevent="isLogin = !isLogin">
          {{ isLogin ? 'Sign Up' : 'Login' }}
        </a>
      </p>
    </form>
  </div>

  <div v-else class="auth-container profile-container">
    <div class="profile-header">
      <div class="profile-photo">
        <img src="https://www.gravatar.com/avatar/00000000000000000000000000000000?d=mp&f=y" alt="Profile" />
      </div>
      <h2>{{ currentUser }}</h2>
    </div>
    <button @click="handleLogout" class="logout-button">Log Out</button>
  </div>
</template>

<style scoped>
.auth-container {
  width: 350px;
  margin: 15vh auto;
  padding: 2rem;
  border-radius: 8px;
  background-color: #1c1c1c;
}

.auth-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  width: 100%;
}

.form-group input {
  width: 85%; 
  padding: 0.75rem;
  border: none;
  border-radius: 4px;
  font-size: 0.9rem;
  background-color: #2b2b2b;
  color: #ffffff;
}

button {
  width: 85%;
  padding: 0.75rem;
  margin-top: 0.5rem;
  background-color: #2ea043;
}

.btn-container {
  display: flex;
  justify-content: center;
}

.toggle-text {
  color: #ffffff;
  font-size: 0.9rem;
}

.toggle-text a {
  color: #2ea043;
}

h1 {
  color: #539bf5;
  font-size: 1.75rem;
  margin-bottom: 1.5rem;
}

.error-message {
  background-color: #ff444444;
  color: #ff6666;
  padding: 0.75rem;
  border-radius: 4px;
  text-align: center;
  margin-bottom: 1rem;
}

.success-message {
  background-color: #2ea04344;
  color: #2ea043;
  padding: 0.75rem;
  border-radius: 4px;
  text-align: center;
  margin-bottom: 1rem;
}

.form-group {
  display: flex;
  justify-content: center;
  width: 100%;
}

.profile-container {
  text-align: center;
}

.profile-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  margin-bottom: 2rem;
}

.profile-photo {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid #2ea043;
}

.profile-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

h2 {
  color: #ffffff;
  font-size: 1.5rem;
  margin: 0;
}

.logout-button {
  background-color: #ff4444;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 0.75rem;
  width: 85%;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.logout-button:hover {
  background-color: #ff2222;
}
</style>
