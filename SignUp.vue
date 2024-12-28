<template>
  <div class="style-class">
    <h2>Sign Up</h2>
    <form @submit.prevent="signUp">
      <div>
        <label for="username">Username:</label>
        <input v-model="username" type="text" id="username" required />
      </div>
      <div>
        <label for="email">Email:</label>
        <input v-model="email" type="email" id="email" required />
      </div>
      <div>
        <label for="password">Password:</label>
        <input v-model="password" type="password" id="password" required />
      </div>
      <button type="submit">Sign Up</button>
    </form>

    <div v-if="errorMessage" class="error">
      <p>{{ errorMessage }}</p>
    </div>

    <div v-if="successMessage" class="success">
      <p>{{ successMessage }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      username: '',
      email: '',
      password: '',
      errorMessage: '',
      successMessage: ''
    };
  },
  methods: {
    async signUp() {
      try {
        const response = await axios.post('http://localhost:8000/signup/', {
          username: this.username,
          password: this.password,
          email: this.email,
        });

        
        const token = response.data.access;  

        if (token) {
          localStorage.setItem('token', token);
          this.successMessage = 'Sign-up successful! Please log in.';
        } else {
          this.successMessage = 'Sign-up successful!';
        }

        this.errorMessage = '';
      } catch (error) {
        this.errorMessage = error.response.data.error || 'Error signing up. Please try again.';
        this.successMessage = '';
      }
    }
  }
};
</script>

<!-- <style scoped>
.error {
  color: red;
  font-size: 1.2rem;
  text-align: center;
}

.success {
  color: green;
  font-size: 1.2rem;
  text-align: center;
}
</style> -->

<style scoped>
/* Page layout */
.style-class {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background-color: #f5f5f5;
  font-family: 'Arial', sans-serif;
}

/* Heading */
h2 {
  font-size: 2rem;
  color: #333;
  margin-bottom: 1rem;
  text-align: center;
}

/* Form styling */
form {
  background: #ffffff;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 360px;
}

/* Labels */
label {
  font-size: 0.9rem;
  color: #555;
  margin-bottom: 0.5rem;
  display: block;
}

/* Input fields */
input[type='text'],
input[type='email'],
input[type='password'] {
  width: 100%;
  padding: 0.5rem;
  margin-bottom: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1rem;
  box-sizing: border-box;
}

input:focus {
  border-color: #4a90e2;
  outline: none;
}

/* Submit button */
button[type='submit'] {
  width: 100%;
  padding: 0.6rem;
  font-size: 1rem;
  color: #ffffff;
  background-color: #4a90e2;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button[type='submit']:hover {
  background-color: #357abd;
}

/* Messages */
.error,
.success {
  margin-top: 1rem;
  padding: 0.5rem;
  border-radius: 4px;
  text-align: center;
}

.error {
  background-color: #ffe6e6;
  color: #d9534f;
}

.success {
  background-color: #e6ffe6;
  color: #5cb85c;
}
</style>
