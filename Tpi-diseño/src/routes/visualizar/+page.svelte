<script>
    import { goto } from "$app/navigation";
    // Arreglo de patentes (RTOs) hardcodeadas
    import { onMount } from "svelte";
    import Calendario from "$lib/Components/tabla.svelte";
    let username = "Usuario";
    let turnos = [
        {
            fecha: "2023-10-01",
            hora: "10:00",
            descripcion: "Turno 1",
            cliente: "joaquin",
            vehiculo: "aveo",
        },
        {
            fecha: "2023-10-02",
            hora: "11:00",
            descripcion: "Turno 2",
            cliente: "joaquin",
            vehiculo: "aveo",
        },
        // Agrega más turnos aquí
    ];
    // Recuperar el usuario desde localStorage al cargar la página
    onMount(() => {
        username = localStorage.getItem("loggedInUser") || "Usuario";
    });
</script>

<header>
    <div class="user-info">
        <span>{username}</span>
        <div class="icon">U</div>
    </div>
    <button class="home-button" on:click={() => goto("/Home")}>Home</button>
</header>
<Calendario {turnos} />

<style>
    .form-container {
        background-color: #7b7a7c;
        padding: 40px;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 50px auto;
        max-width: 600px;
        display: block;
    }

    .form-container h2 {
        margin-bottom: 20px;
    }

    .form-container label {
        margin-top: 10px;
    }

    .form-container input,
    .form-container select {
        margin-bottom: 10px;
        padding: 8px;
        width: 100%;
        max-width: 300px;
        border-radius: 5px;
        border: 1px solid #ccc;
    }

    .form-container button {
        margin-top: 20px;
        padding: 10px 20px;
        background-color: #4caf50;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }

    .form-container button:hover {
        background-color: #45a049;
    }

    .factura-container {
        background-color: #d0cfd1;
        padding: 40px;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 50px auto;
        max-width: 600px;
    }

    .factura-container h2 {
        margin-bottom: 20px;
    }

    .factura-container p {
        margin: 10px 0;
    }

    .factura-container button {
        background-color: #4caf50;
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
        cursor: pointer;
    }
    form div {
        display: flex;
        flex-direction: column;
        margin-bottom: 1rem;
    }

    label {
        margin-bottom: 0.5rem;
        font-weight: bold;
    }

    input {
        padding: 0.5rem;
        font-size: 1rem;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    header {
        width: 100%;
        display: flex;
        margin-right: auto;
        padding: 1rem 2rem;
        background-color: #f5f5f5;
        border-bottom: 1px solid #ddd;
        box-sizing: border-box;
    }

    .home-button {
        margin-left: auto;
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

    .user-info {
        display: flex;
        align-items: center;
        gap: 0.5rem;
    }
</style>
