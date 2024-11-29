<script>
    import { goto } from '$app/navigation';
    let username = 'Usuario';
  
    // Puedes reemplazar este valor dinámicamente en el futuro.
    $: userInitial = username.charAt(0).toUpperCase();
  </script>
  
  <style>
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem;
      background-color: #6c9fcc; /* Azul suave */
      color: white;
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
      background-color: white;
      color: #6c9fcc;
      font-size: 1.2rem;
      font-weight: bold;
    }
  
    .home-button {
      background-color: white;
      color: #6c9fcc;
      border: none;
      border-radius: 5px;
      padding: 0.5rem 1rem;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
  
    .home-button:hover {
      background-color: #4a88aa; /* Azul más oscuro */
      color: white;
    }
  </style>
  
  <header>
    <div class="user-info">
      <span>{username}</span>
      <div class="icon">{userInitial}</div>
    </div>
    <button class="home-button" on:click={() => goto('/Home')}>Home</button>
  </header>
  
  <slot /> <!-- Aquí se renderiza el contenido específico de cada página -->
  