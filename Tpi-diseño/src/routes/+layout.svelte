<script>
  // Opcional: Manejar la navegación actual si lo necesitas
  let currentPath = "/";
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

  nav ul {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
    gap: 1.5rem;
  }

  nav ul li {
    margin: 0;
  }

  nav ul li a {
    text-decoration: none;
    font-size: 1rem;
    color: #0056b3;
    font-weight: 500;
    transition: color 0.3s ease;
  }

  nav ul li a:hover {
    color: #007bff;
  }

  footer {
    background-color: #f5f5f5;
    text-align: center;
    padding: 1rem;
    color: #0056b3;
    font-size: 0.9rem;
    box-shadow: 0 -2px 5px rgba(0, 0, 0, 0.1);
  }

</style>

<nav>
  <a href="/" class="logo">RTO</a>
  <ul>
    <li><a href="/" class="{currentPath === '/' ? 'active' : ''}">Inicio</a></li>
    <li><a href="/about" class="{currentPath === '/about' ? 'active' : ''}">Sobre Nosotros</a></li>
    <li><a href="/login" class="{currentPath === '/login' ? 'active' : ''}">Iniciar Sesión</a></li>
    <li><a href="/register" class="{currentPath === '/register' ? 'active' : ''}">Registrarse</a></li>
  </ul>
</nav>

<slot />

<footer>
  © 2024 Desarrollado con 💙 por el Grupo 8.
</footer>
