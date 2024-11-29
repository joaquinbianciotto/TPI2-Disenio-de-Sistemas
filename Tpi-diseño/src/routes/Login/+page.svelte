<script>
    import { goto } from '$app/navigation';
  
    /**
     * Simula la validación del inicio de sesión.
     * @param {string} username
     * @param {string} password
     * @returns {boolean} true si es válido, false si no.
     */
    function login(username, password) {
      // Sustituir con la lógica de validación real.
      return username === 'admin' && password === '1234';
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

  <style> 
    
    form {
      max-width: 300px;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
  
    label {
      display: block;
      margin-bottom: 10px;
      width: 100%;
    }
  
    input {
      width: 100%;
      padding: 5px;
      margin-top: 5px;
    }
  
    button {
      display: block;
      margin-top: 10px;
      padding: 5px 10px;
      background-color: #0044ff;
      color: white;
      border: none;
      cursor: pointer;
    }
  
    button:hover {
      background-color: #1b00b3;
    }
  
    .login-form {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .login-form input{
      margin: 10px 0;
    }

    h1 {
      text-align: center;
      margin-top: 100px;
      width: 100%;
    }

    .form-conatainer{
      background-color: #7b7a7c;
      padding: 40px;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      display: flex;
      flex-direction: column;
      align-items: center;
      margin: auto ;
    }
   
  </style>

    <form class='login-form' on:submit|preventDefault={handleSubmit}>
        <h1>Titulo de la pagina</h1>
        <div class='form-conatainer'>    
        <h1>Iniciar Sesión</h1>
        <label>
            Usuario:
            <input type="text" bind:value={username} placeholder="Username" />
        </label>
        <br />
        <label>
        Contraseña:
            <input type="password" bind:value={password} placeholder="Password" />
        </label>
        <br />
        <button type="submit">Login</button>
        </div>
    </form>
  