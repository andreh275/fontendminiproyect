<template>
  <div class="login-container">
    <div class="promo-message">
      <h2>¡Bienvenido a nuestra plataforma!</h2>
      <p>Únete ahora y disfruta de nuestros increíbles servicios exclusivos.</p>
      <p>¡Iniciar sesión y comenzar es muy fácil!</p>
    </div>
    
    <h1>Iniciar sesión</h1>
    <form @submit.prevent="handleLogin">
      <div class="input-container">
        <label for="username">Usuario:</label>
        <input type="text" id="username" v-model="username" required />
      </div>
      <div class="input-container">
        <label for="password">Contraseña:</label>
        <input type="password" id="password" v-model="password" required />
      </div>
      <div class="input-container">
        <label for="userType">Tipo de usuario:</label>
        <select id="userType" v-model="userType" required>
          <option value="" disabled selected>Seleccione un tipo</option>
          <option value="cliente">Cliente</option>
          <option value="proveedor">Proveedor</option>
        </select>
      </div>
      <button type="submit" class="login-button">Iniciar sesión</button>
    </form>
    <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p> <!-- Mostramos el error aquí -->
  </div>
</template>

<script>
import { useAuthStore } from '../stores/authStore.js';

export default {
  data() {
    return {
      username: '',
      password: '',
      userType: '',
    };
  },
  computed: {
    errorMessage() {
      const authStore = useAuthStore();
      return authStore.errorMessage; // Obtenemos el mensaje de error del store
    }
  },
  methods: {
    handleLogin() {
      const authStore = useAuthStore();
      authStore.login({ username: this.username, password: this.password }, this.userType);

      // Redirige según el tipo de usuario seleccionado
      if (authStore.isAuthenticated) {
        if (this.userType === 'cliente') {
          this.$router.push('/home-clientes');
        } else if (this.userType === 'proveedor') {
          this.$router.push('/home');
        }
      }
    }
  }
};
</script>

<style scoped>
.login-container {
  max-width: 450px;
  margin: 50px auto;
  padding: 30px;
  background-color: rgba(255, 255, 255, 0.8);
  border: 2px solid #000000;
  border-radius: 10px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
  background-image: url('https://your-image-url.com'); /* Inserta la URL de la imagen de fondo */
  background-size: cover;
  background-position: center;
  position: relative;
  text-align: center;
  color: url('#000000');
}
.promo-message {
  background-color: rgba(0, 0, 0, 0.6);
  color: white;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 20px;
}

h1 {
  font-family: 'Arial', sans-serif;
  color: #2196F3;
  font-size: 24px;
  margin-bottom: 20px;
}

.input-container {
  margin-bottom: 15px;
}

input, select {
  width: 100%;
  padding: 12px;
  margin-top: 5px;
  border-radius: 5px;
  border: 1px solid #ddd;
  transition: border 0.3s ease-in-out;
}

input:focus, select:focus {
  border-color: #2196F3;
  outline: none;
}

button {
  padding: 12px 20px;
  background-color: #2196F3;
  color: white;
  border: none;
  cursor: pointer;
  width: 100%;
  border-radius: 5px;
  transition: background-color 0.3s ease-in-out;
}

button:hover {
  background-color: #1976D2;
}

.error-message {
  color: red;
  margin-top: 10px;
  font-size: 14px;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.login-container {
  animation: fadeIn 1s ease-in-out;
}
</style>
