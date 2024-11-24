<template>
  <div class="login-container">
    <h2>Login</h2>
    <form @submit.prevent="handleLogin" class="login-form">
      <div class="form-group">
        <label for="username">Username</label>
        <input
            v-model="username"
            type="text"
            id="username"
            name="username"
            required
            placeholder="Enter your username" />
      </div>

      <div class="form-group">
        <label for="password">Password</label>
        <input
            v-model="password"
            type="password"
            id="password"
            name="password"
            required
            placeholder="Enter your password" />
      </div>

      <button type="submit" class="login-button">Login</button>

      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
      <p v-if="successMessage" class="success-message">{{ successMessage }}</p>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      username: '',
      password: '',
      errorMessage: '',
      successMessage: ''
    };
  },
  methods: {
    async handleLogin() {
      // Clear previous messages
      this.errorMessage = '';
      this.successMessage = '';

      // Basic validation
      if (!this.username || !this.password) {
        this.errorMessage = 'Please enter both username and password.';
        return;
      }

      try {
        // Send POST request to the login API endpoint
        const response = await axios.post('https://localhost:7102/api/Users/login', {
          Id: "dummy-id",  // Add a dummy Id field if necessary
          username: this.username,
          password: this.password
        });

        // Handle successful login
        if (response.data.success) {
          this.successMessage = response.data.message;
        } else {
          this.errorMessage = 'Invalid username or password.';
        }
      } catch (error) {
        console.error('Login error:', error);
        this.errorMessage = 'An error occurred while logging in. Please try again.';
      }
    }
  }
};
</script>

<style scoped>
/* Genel Stil */
.login-container {
  max-width: 100%;
  width: 100%;
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
  box-sizing: border-box;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
}

.login-form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 15px;
}

label {
  font-size: 1rem;
  margin-bottom: 5px;
}

input {
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
  box-sizing: border-box;
}

button {
  padding: 10px;
  font-size: 1rem;
  border: none;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
  border-radius: 4px;
  margin-top: 15px;
}

button:hover {
  background-color: #45a049;
}

.error-message, .success-message {
  text-align: center;
  font-size: 0.9rem;
  margin-top: 10px;
}

.error-message {
  color: red;
}

.success-message {
  color: green;
}

/* Responsive */
@media (max-width: 768px) {
  .login-container {
    padding: 15px;
  }

  h2 {
    font-size: 1.5rem;
  }

  input {
    padding: 12px;
    font-size: 1.1rem;
  }

  button {
    padding: 12px;
    font-size: 1.1rem;
  }

  .form-group {
    margin-bottom: 12px;
  }
}

@media (max-width: 480px) {
  .login-container {
    padding: 10px;
  }

  h2 {
    font-size: 1.2rem;
  }

  input {
    padding: 10px;
    font-size: 1rem;
  }

  button {
    padding: 10px;
    font-size: 1rem;
  }
}
</style>
