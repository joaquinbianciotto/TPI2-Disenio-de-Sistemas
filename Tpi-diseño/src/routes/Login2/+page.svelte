<script>
  import { goto } from '$app/navigation';

  // Variables for user input and error messages
  let username = '';
  let password = '';
  let showPassword = false;

  // Error messages for fields
  let errors = {
    username: '',
    password: '',
  };

  // Success message for password recovery
  let recoveryMessage = '';

  function login(username, password) {
    return username === 'admin' && password === '1234';
  }

  const handleSubmit = () => {
    let isValid = true;

    // Clear previous errors and messages
    errors = { username: '', password: '' };
    recoveryMessage = '';

    if (username.trim().length === 0) {
      errors.username = 'El CUIT/DNI es obligatorio.';
      isValid = false;
    }

    if (password.trim().length === 0) {
      errors.password = 'La contraseña es obligatoria.';
      isValid = false;
    }

    if (isValid && login(username, password)) {
      localStorage.setItem('loggedInUser', username);
      goto('/Home');
    } else if (isValid) {
      errors.password = 'Credenciales incorrectas.';
    }
  };

  const handleForgotPassword = () => {
    // Clear previous errors and show recovery message
    errors = { username: '', password: '' };
    recoveryMessage = 'Por favor, revise su correo electrónico para instrucciones de recuperación.';
  };

  const handleRegister = () => {
    goto('/Registrar');
  };

  const togglePasswordVisibility = () => {
    showPassword = !showPassword;
  };
</script>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
  }

  .page-container {
    font-family: 'Arial', sans-serif;
    background: linear-gradient(135deg, #1e5799 0%, #2989d8 50%, #207cca 100%);
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    padding: 0;
    overflow: hidden;
  }

  .login-container {
    background: white;
    border-radius: 10px;
    width: 400px;
    padding: 30px;
    text-align: center;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  }

  .login-container h1 {
    margin: 0;
    margin-bottom: 20px;
    font-size: 24px;
    color: black;
  }

  .input-group {
    display: flex;
    align-items: center;
    margin: 10px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
    overflow: hidden;
    background: white;
  }

  .input-group input {
    flex: 1;
    padding: 10px;
    border: none;
    font-size: 16px;
    outline: none;
  }

  .input-group .icon {
    padding: 10px;
    background: #f0f0f0;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #888;
  }

  .login-container button {
    width: 80%;
    padding: 12px;
    border: none;
    border-radius: 5px;
    background-color: #007bff;
    color: white;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .login-container button:hover {
    background-color: #0056b3;
  }

  .password-eye {
    cursor: pointer;
    color: #888;
  }

  .login-container a {
    text-decoration: none;
    color: #007bff;
    font-size: 14px;
    margin-top: 10px;
    display: inline-block;
    cursor: pointer;
  }

  .login-container a:hover {
    text-decoration: underline;
  }

  .login-container .footer {
    margin-top: 20px;
  }

  /* New styles for messages */
  .error {
    color: red;
    font-size: 12px;
    margin-top: 5px;
    text-align: left;
  }

  .message {
    color: green;
    font-size: 14px;
    margin-top: 15px;
  }
</style>

<div class="page-container">
  <div class="login-container">
    <h1>Iniciar Sesión</h1>
    <div class="input-group">
      <span class="icon">👤</span>
      <input
        type="text"
        placeholder="CUIT/DNI"
        bind:value={username}
      />
    </div>
    {#if errors.username}
      <div class="error">{errors.username}</div>
    {/if}

    <div class="input-group">
      <span class="icon">🔑</span>
      <input
        type={showPassword ? 'text' : 'password'}
        placeholder="Contraseña"
        bind:value={password}
      />
      <span class="icon password-eye" on:click={togglePasswordVisibility}>
        {showPassword ? '🙈' : '👁️'}
      </span>
    </div>
    {#if errors.password}
      <div class="error">{errors.password}</div>
    {/if}

    <a on:click|preventDefault={handleForgotPassword}>¿Olvidó su contraseña?</a>
    <button on:click={handleSubmit}>Ingresar</button>
    {#if recoveryMessage}
      <div class="message">{recoveryMessage}</div>
    {/if}
    <div class="footer">
      <p>¿No sos un miembro? <a href="/Registrar" on:click|preventDefault={handleRegister}>Registrarse</a></p>
    </div>
  </div>
</div>
