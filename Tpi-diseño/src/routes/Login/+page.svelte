<script>
  import { goto } from '$app/navigation';
  
  /**
   * Simula la validación del inicio de sesión.
   * @param {string} username
   * @param {string} password
   * @returns {boolean} true si es válido, false si no.
   */
  function login(username, password) {
    
    return true;
  }
  
  let username = '';
  let password = '';
  
  const handleSubmit = () => {
    if (login(username, password)) {
      localStorage.setItem('loggedInUser', username);
      goto('/Home'); // Redirige a la página principal.
    } else {
      alert('Credenciales incorrectas');
    }
  };
</script>

<!-- Bootstrap CDN -->
<link
  href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha3/dist/css/bootstrap.min.css"
  rel="stylesheet"
/>

<style>
  body {
    background-color: #f7f7f7; /* Fondo gris claro */
  }


  .login-container {
    max-width: 400px;
    background-color: #7b7a7c; /* Fondo gris oscuro */
    border-radius: 10px;
    padding: 40px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
    color: white;
  }

  .btn-primary {
    background-color: #0044ff; /* Azul primario */
    border: none;
  }

  .btn-primary:hover {
    background-color: #0033cc; /* Azul más oscuro */
  }

  .form-control {
    background-color: #e0e0e0; /* Gris claro */
    border: 1px solid #ccc;
    color: #333;
  }

  h1 {
    text-align: center;
    color: white;
  }
</style>


<div class="d-flex align-items-center justify-content-center vh-100">
  <div class="login-container">
    <h1>Iniciar Sesión</h1>
    <form on:submit|preventDefault={handleSubmit}>
      <div class="mb-3">
        <label for="username" class="form-label">Usuario</label>
        <input
          type="text"
          id="username"
          class="form-control"
          bind:value={username}
          placeholder="Ingrese su usuario"
          required
        />
      </div>
      <div class="mb-3">
        <label for="password" class="form-label">Contraseña</label>
        <input
          type="password"
          id="password"
          class="form-control"
          bind:value={password}
          placeholder="Ingrese su contraseña"
          required
        />
      </div>
      <button type="submit" class="btn btn-primary w-100">Iniciar Sesión</button>
    </form>
  </div>
</div>
