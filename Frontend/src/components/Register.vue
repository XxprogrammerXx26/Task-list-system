<template>
    <div class="register-container">
      <h1>Register</h1>
      
      <!-- Formulario de registro -->
      <form @submit.prevent="handleRegister" class="register-form">
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
          <label for="email">Email</label>
          <input 
            type="email" 
            id="email" 
            v-model="email" 
            required 
            placeholder="Enter your email" 
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
  
        <button type="submit" class="register-btn">Register</button>
      </form>
  
      <!-- Enlace de texto para volver al login -->
      <p class="back-link" @click="goBack">Already have an account? Go back to login</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        username: '',
        email: '',
        password: '',
      };
    },
    methods: {
      handleRegister() {
        // Simulación de registro, aquí puedes integrar tu lógica real de registro
        console.log('User registered:', this.username, this.email, this.password);
        this.$router.push('/login'); // Redirigir a la página de login después de registrarse
      },
      goBack() {
        this.$router.go(-1);  // Usar el historial de Vue Router para volver a la página anterior
      }
    }
  };
  </script>
  
  <style scoped>
  /* Estilos para el contenedor y el formulario de registro */
  .register-container {
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
  
  .register-form {
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
  
  /* Estilo del enlace de texto */
  .back-link {
    margin-top: 15px;
    font-size: 1em;
    color: #007bff;
    cursor: pointer;
    text-decoration: underline;
  }
  
  .back-link:hover {
    color: #0056b3;
  }
  </style>
  