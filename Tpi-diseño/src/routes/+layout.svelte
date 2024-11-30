<script lang="ts">
  import { page } from '$app/stores';
  import { goto } from '$app/navigation';
  import { onMount, onDestroy } from 'svelte';

  let currentPath = '';
  let username = 'Usuario';

  $: {
    const unsubscribe = page.subscribe(($page) => {
      currentPath = $page.url.pathname;
    });
    onDestroy(() => {
      unsubscribe();
    });
  }
  
  onMount(() => {
    username = localStorage.getItem("loggedInUser") || "Usuario";
  });

  function navigateTo(path: string) {
        goto(path).then(() => {
            currentPath = path;
        });
    }
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
    margin-top: 0;
  }

  main{
    flex: 1 ;
  }

  .page-container {
    display: flex;
    flex-direction: column;
    min-height: 100vh; 
  }

  .user-info {
    display: flex;
    align-items: center;
  }

  .user-info img {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    margin-right: 0.5rem;
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


  /*estilos para otros tipos de pantallas*/

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

<div class="page-container">
  <nav>
    <a href="/Home" class="logo"><img src="/rto-movil.png" alt="logo"></a>
    <div class="buttons">
      
      {#if currentPath == '/HomePublico'}
        <button on:click={() =>  goto('/Login')}>Iniciar Sesión</button>
      {/if}
    </div>

      {#if currentPath !== '/Login' && currentPath !== '/HomePublico'}
        <div class="user-info">
          <img src="/src/user-icon.png" alt="User Icon" />
          <span>{"Personal de atencion al cliente"}</span>
        </div>
      {/if}

  </nav>

  <main>
    <slot />
  </main>

  <footer>
    <div class="footer-content">
      <p>© 2024 Desarrollado con 💙 por el Grupo 8.</p>
      <p>Contacto: grupo8@gmail.com</p>
    </div>
  </footer>
</div>

