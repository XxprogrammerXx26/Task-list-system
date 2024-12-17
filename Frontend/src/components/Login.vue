<template>
  <div class="login-container">
    <h1>Login</h1>
    
    <!-- Formulario de login -->
    <form @submit.prevent="handleLogin" class="login-form">
      <div class="form-group">
        <label for="username">Username</label>
        <input 
          type="text" 
          id="username" 
          v-model="username" 
          required 
          placeholder="Enter your username" 
        />
      </div>
      
      <div class="form-group">
        <label for="password">Password</label>
        <input 
          type="password" 
          id="password" 
          v-model="password" 
          required 
          placeholder="Enter your password" 
        />
      </div>
      
      <button type="submit" class="login-btn">Login</button>
    </form>

    <!-- Mensaje de error si las credenciales son incorrectas -->
    <p v-if="loginError" class="error-message">Invalid username or password</p>

    <!-- Enlace a la página de registro si el usuario no tiene cuenta -->
    <p class="register-link">
      Don't have an account? 
      <router-link to="/register" class="register-link">Register here</router-link>
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: '',
      loginError: false,  // Para mostrar el mensaje de error si las credenciales son incorrectas
    };
  },
  methods: {
    handleLogin() {
      // Simulación de autenticación, aquí puedes integrar tu lógica real de login
      if (this.username === 'leo' && this.password === '12345') {
        this.$router.push('/tasklist'); // Redirigir a la página de dashboard si es exitoso
      } else {
        this.loginError = true; // Mostrar el error si las credenciales son incorrectas
      }
    }
  }
};
</script>

<style scoped>

.login-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f4f4f4;
}

h1 {
  font-size: 2em;
  margin-bottom: 20px;
  color: #333;
}

.login-form {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  width: 300px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  font-size: 1em;
  color: #333;
}

input {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border-radius: 4px;
  border: 1px solid #ccc;
  font-size: 1em;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
}

button:hover {
  background-color: #45a049;
}

.error-message {
  color: red;
  font-size: 0.9em;
  margin-top: 20px;
}

/* Estilos para el enlace de registro */
.register-link {
  margin-top: 15px;
  font-size: 1em;
  color: #333;
}

.register-link a {
  color: #4CAF50;
  text-decoration: none;
}

.register-link a:hover {
  text-decoration: underline;
}
</style>
