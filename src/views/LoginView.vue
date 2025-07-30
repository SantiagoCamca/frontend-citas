<template>
  <div class="login-page d-flex justify-content-center align-items-center">
    <div class="login-box shadow rounded p-4">
      <h2 class="text-center mb-4">Iniciar Sesión</h2>

      <form @submit.prevent="iniciarSesion" novalidate>
        <div class="mb-3">
          <label for="email" class="form-label">Correo electrónico</label>
          <input
            id="email"
            type="email"
            class="form-control"
            v-model="correo"
            placeholder="tu@correo.com"
            required
          />
        </div>

        <div class="mb-4">
          <label for="password" class="form-label">Contraseña</label>
          <input
            id="password"
            type="password"
            class="form-control"
            v-model="contrasena"
            placeholder="********"
            required
          />
        </div>

        <button type="submit" class="btn btn-gradient w-100 fw-bold">
          Entrar
        </button>
      </form>

      <p v-if="error" class="text-danger mt-3 text-center">{{ error }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const correo = ref('')
const contrasena = ref('')
const error = ref(null)

const iniciarSesion = async () => {
  try {
    const res = await fetch('https://backend-citas-production.up.railway.app/api/Auth/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ correo: correo.value, contrasena: contrasena.value })
    })

    if (!res.ok) throw new Error('Credenciales incorrectas')

    const data = await res.json()
    localStorage.setItem('token', data.token)
    alert('Login exitoso')
    // Aquí puedes redirigir o mostrar la siguiente vista
  } catch (err) {
    error.value = err.message
  }
}
</script>

<style scoped>
.login-page {
  min-height: 100vh;
  width: 100vw;
  background: linear-gradient(135deg, #667eea, #764ba2);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  box-sizing: border-box;
}

.login-box {
  background: white;
  max-width: 400px;
  width: 100%;
  border-radius: 12px;
  padding: 2rem 2.5rem;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

.btn-gradient {
  background: linear-gradient(45deg, #6a11cb, #2575fc);
  border: none;
  color: white;
  padding: 0.6rem 0;
  font-size: 1.1rem;
  border-radius: 50px;
  transition: background 0.3s ease;
}

.btn-gradient:hover {
  background: linear-gradient(45deg, #2575fc, #6a11cb);
  color: white;
  box-shadow: 0 4px 15px rgba(101, 67, 255, 0.5);
}
</style>
