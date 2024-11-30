<script>
  import { goto } from '$app/navigation';

  // Variables for form data and error messages
  let cuil = '';
  let firstName = '';
  let lastName = '';
  let phone = '';
  let email = '';
  let password = '';
  let showPassword = false;

  // Error messages for each field
  let errors = {
    cuil: '',
    firstName: '',
    lastName: '',
    phone: '',
    email: '',
    password: '',
  };

  // Toggle password visibility
  const togglePasswordVisibility = () => {
    showPassword = !showPassword;
  };

  // Validate form data
  const validateForm = () => {
    let isValid = true;

    // Clear previous errors
    errors = { cuil: '', firstName: '', lastName: '', phone: '', email: '', password: '' };

    if (!/^\d{7,11}$/.test(cuil)) {
      errors.cuil = 'El CUIL/DNI debe ser un número válido entre 7 y 11 dígitos.';
      isValid = false;
    }

    if (firstName.trim().length < 2) {
      errors.firstName = 'El nombre debe tener al menos 2 caracteres.';
      isValid = false;
    }

    if (lastName.trim().length < 2) {
      errors.lastName = 'El apellido debe tener al menos 2 caracteres.';
      isValid = false;
    }

    if (!/^\d{7,15}$/.test(phone)) {
      errors.phone = 'El número de teléfono debe contener entre 7 y 15 dígitos.';
      isValid = false;
    }

    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
      errors.email = 'Por favor, ingrese un correo electrónico válido.';
      isValid = false;
    }

    if (password.trim().length < 8) {
      errors.password = 'La contraseña debe tener al menos 8 caracteres.';
      isValid = false;
    }

    return isValid;
  };

  // Handle form submission
  const handleSubmit = () => {
    if (validateForm()) {
      console.log('Datos registrados:', { cuil, firstName, lastName, phone, email, password });
      alert('Registro exitoso');
      goto('/Login2');
    }
  };

  // Navigate back to login
  const handleBackToLogin = () => {
    goto('/Login2');
  };
</script>


<style>
  :global(body) {
    margin: 0;
    padding: 0;
  }

  .error {
    color: red;
    font-size: 12px;
    margin-top: 5px;
    text-align: left;
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

  .register-container {
    background: white;
    border-radius: 10px;
    width: 400px;
    padding: 30px;
    text-align: center;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  }

  .register-container h1 {
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

  .password-eye {
    cursor: pointer;
    color: #888;
  }

  .register-container button {
    width: 80%;
    padding: 12px;
    margin-top: 20px;
    border: none;
    border-radius: 5px;
    background-color: #007bff;
    color: white;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .register-container button:hover {
    background-color: #0056b3;
  }

  .register-container .footer {
    margin-top: 20px;
    font-size: 14px;
  }

  .register-container .footer a {
    color: #007bff;
    text-decoration: none;
  }

  .register-container .footer a:hover {
    text-decoration: underline;
  }
</style>


<div class="page-container">
  <div class="register-container">
    <h1>Crear Cuenta</h1>
    <div class="input-group">
      <span class="icon">🆔</span>
      <input type="text" placeholder="Ingrese su CUIL o DNI" bind:value={cuil} />
    </div>
    {#if errors.cuil}
      <div class="error">{errors.cuil}</div>
    {/if}

    <div class="input-group">
      <span class="icon">👤</span>
      <input type="text" placeholder="Ingrese su nombre" bind:value={firstName} />
    </div>
    {#if errors.firstName}
      <div class="error">{errors.firstName}</div>
    {/if}

    <div class="input-group">
      <span class="icon">👥</span>
      <input type="text" placeholder="Ingrese su apellido" bind:value={lastName} />
    </div>
    {#if errors.lastName}
      <div class="error">{errors.lastName}</div>
    {/if}

    <div class="input-group">
      <span class="icon">📞</span>
      <input type="text" placeholder="Ingrese su número de teléfono" bind:value={phone} />
    </div>
    {#if errors.phone}
      <div class="error">{errors.phone}</div>
    {/if}

    <div class="input-group">
      <span class="icon">✉️</span>
      <input type="email" placeholder="Ingrese su correo electrónico" bind:value={email} />
    </div>
    {#if errors.email}
      <div class="error">{errors.email}</div>
    {/if}

    <div class="input-group">
      <span class="icon">🔑</span>
      <input type={showPassword ? 'text' : 'password'} placeholder="Ingrese su contraseña" bind:value={password} />
      <span class="icon password-eye" on:click={togglePasswordVisibility}>
        {showPassword ? '🙈' : '👁️'}
      </span>
    </div>
    {#if errors.password}
      <div class="error">{errors.password}</div>
    {/if}

    <button on:click={handleSubmit}>Registrar</button>
    <div class="footer">
      <p>
        ¿Ya tienes una cuenta?
        <a href="/Login2" on:click|preventDefault={handleBackToLogin}>Inicia sesión aquí</a>
      </p>
    </div>
  </div>
</div>





