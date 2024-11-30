<script>

  import { goto } from "$app/navigation";
  import { onMount } from "svelte";

  let username = "Usuario"; // Valor por defecto del usuario

  // Recuperar el usuario desde localStorage al cargar la página
  onMount(() => {
    username = localStorage.getItem("loggedInUser") || "Usuario";
  });

  /**
   * Manejo de las acciones de los botones
   * @param {string} action Acción seleccionada
   */
  const handleAction = (action) => {
    console.log(`Acción seleccionada: ${action}`);
  };
</script>

<main>
  <div class="button-group">
    <button on:click={() => goto("/facturacion")}>Facturacion</button>
    <button on:click={() => goto("/visualizar")}>Ver Turnos</button>
  </div>
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    text-align: center;
    align-items: center;
    height: 60vh;
    width: 100%;
  }

  .button-group {
    display: flex;
    flex-direction: column;
    gap: 1rem;
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

