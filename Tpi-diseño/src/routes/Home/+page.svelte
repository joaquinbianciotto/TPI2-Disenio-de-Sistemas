<script>
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';

  let username = 'Usuario'; // Valor por defecto del usuario

  // Recuperar el usuario desde localStorage al cargar la página
  onMount(() => {
    username = localStorage.getItem('loggedInUser') || 'Usuario';
  });

  /**
   * Manejo de las acciones de los botones
   * @param {string} action Acción seleccionada
   */
  const handleAction = (action) => {
    console.log(`Acción seleccionada: ${action}`);
  };
</script>

<style>
 
 header {
    width: 100%;
    display: flex;
    justify-content: flex-end; 
    align-items: center;
    padding: 1rem 2rem;
    background-color: #f5f5f5;
    border-bottom: 1px solid #ddd;
    box-sizing: border-box;
  }


  .user-info {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .icon {
    width: 35px;
    height: 35px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    background-color: #007bff; 
    color: white;
    font-size: 1.2rem;
    font-weight: bold;
  }

  main {
    text-align: center;
    margin-top: 2rem;
    width: 100%; 
  }

  .button-group {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    align-items: center;
    margin-top: 2rem;
  }

  button {
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    width: 200px;
  }

  button:hover {
    background-color: #0056b3;
  }
</style>

<header>
  <div class="user-info">
    <span>Bienvenido, {username}</span>
    <div class="icon">U</div>
  </div>
</header>


<main>
  <h1>Título de la Página</h1>
  <div class="button-group">
    <button on:click={() => goto('/facturacion')}>Facturacion</button>
    <button on:click={() => goto('modificar turno')}>Modificar Turno</button>
    <button on:click={() => goto('/visualizar')}>Ver Turnos</button>
  </div>
</main>
