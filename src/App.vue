<script setup lang="ts">
import { ref } from 'vue'

const isLogin = ref(true)
const username = ref('')
const password = ref('')

const handleSubmit = () => {
  if (isLogin.value) {
    console.log('Login:', { username: username.value, password: password.value })
  } else {
    console.log('Signup:', { username: username.value, password: password.value })
  }
}
</script>

<template>
  <div class="auth-container">
    <h1>{{ isLogin ? 'Login' : 'Sign Up' }}</h1>
    <form @submit.prevent="handleSubmit" class="auth-form">
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
</style>
