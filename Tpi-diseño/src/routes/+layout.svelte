<script>
  import { page } from '$app/stores';
  import { goto } from '$app/navigation';
  let currentPath = $page.url.pathname; // Ruta actual
</script>

<style>

  nav {
    background-color: #f5f5f5;
    padding: 1rem 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }

  nav .logo {
    font-size: 1.5rem;
    font-weight: bold;
    color: #007bff;
    text-decoration: none;
  }

  nav .buttons {
    display: flex;
    gap: 1rem;
  }

  button {
    padding: auto;
    font-size: 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    width: 100px;
  }

  button:hover {
    background-color: #0056b3;
  }

  footer {
    background-color: #f5f5f5;
    text-align: center;
    padding: 1.5rem;
    color: #0056b3;
    font-size: 0.9rem;
    box-shadow: 0 -2px 5px rgba(0, 0, 0, 0.1);
    margin-top: 2rem;
  }

  .footer-content {
    max-width: 800px;
    margin: 0 auto;
  }

  .footer-content p {
    margin: 0.5rem 0;
  }

  .logo img {
    width: 100%;
    max-width: 200px;
    height: auto;
    display: block;
    margin: 0 auto;
  }

  .logo {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 10px 0;
  }

  /* Media Query para pantallas pequeñas */
  @media (max-width: 768px) {
    nav {
      flex-direction: column;
      padding: 1rem;
    }

    nav .buttons {
      flex-direction: column;
      gap: 0.5rem;
    }

    button {
      width: 100%;
    }

    .footer-content p {
      font-size: 0.8rem;
    }
  }

  /* Media Query para pantallas móviles */
  @media (max-width: 480px) {
    .logo img {
      max-width: 150px;
    }

    footer {
      font-size: 0.8rem;
    }

    button {
      padding: 0.5rem;
    }
  }
</style>

<nav>
  <a href="/Home" class="logo"><img src="/rto-movil.png" alt="logo"></a>
  <div class="buttons">
    <button on:click={() =>  goto('/Home')}>Inicio</button>
    <button on:click={() =>  goto('/About')}>Sobre Nosotros</button>

    {#if currentPath !== '/Login'}
      <button on:click={() =>  goto('/Login')}>Iniciar Sesión</button>
    {/if}

    {#if currentPath !== '/Register'}
      <button on:click={() =>  goto('/Register')}>Registrarse</button>
    {/if}
  </div>
</nav>

<slot />

<footer>
  <div class="footer-content">
    <p>© 2024 Desarrollado con 💙 por el Grupo 8.</p>
    <p>Contacto: grupo8@gmail.com</p>
  </div>
</footer>

