<script>
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';
</script>


<style>
  /* Tus estilos personalizados aquí */
</style>

<header class="header d-flex justify-content-between align-items-center">
  <h5 class="mb-0">Bienvenido, Usuario</h5>
  <div class="user-icon"></div>
</header>
<main class="container text-center">
  <h1 class="my-4">Título de la Página</h1>
  <div class="button-group d-grid gap-3 mx-auto">
    <button
      class="btn btn-lg btn-primary"
      on:click={() => goto('/facturacion')}
    >
      Facturación
    </button>
    <button
      class="btn btn-lg btn-secondary"
      on:click={() => goto('/modificar-turno')}
    >
      Modificar Turno
    </button>
    <button
      class="btn btn-lg btn-info text-white"
      on:click={() => goto('/visualizar')}
    >
      Ver Turnos
    </button>
  </div>
</main>
