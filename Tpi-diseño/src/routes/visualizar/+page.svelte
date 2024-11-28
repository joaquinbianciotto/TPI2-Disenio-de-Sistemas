<script>
    import { goto } from "$app/navigation";
    // Arreglo de patentes (RTOs) hardcodeadas
    import { onMount } from "svelte";
    import Calendario from "$lib/Components/calendario.svelte";
    import Lista from "$lib/Components/lista.svelte";
    import Tabla from "$lib/Components/tabla.svelte";
    let username = "Usuario";
    let turnos = [];
    // Recuperar el usuario desde localStorage al cargar la página
    onMount(() => {
        username = localStorage.getItem("loggedInUser") || "Usuario";
        fetch("/Tpi-diseño/src/lib/datos/turnos.json")
            .then((response) => response.json())
            .then((data) => {
                turnos = data;
            });
    });
    let view = "lista";

    function switchView(selectedView) {
        view = selectedView;
    }
</script>

<header>
    <div class="user-info">
        <span>{username}</span>
        <div class="icon">U</div>
    </div>
    <button class="home-button" on:click={() => goto("/Home")}>Home</button>
</header>

<main>
    <div class="view-selector">
        <button on:click={() => switchView("calendario")}>Ver Calendario</button
        >
        <button on:click={() => switchView("lista")}>Ver Lista</button>
        <button on:click={() => switchView("tabla")}>Ver Tabla</button>
    </div>
    {#if view === "calendario"}
        <Calendario {turnos} />
    {:else if view === "lista"}
        <Lista {turnos} />
    {:else if view === "tabla"}
        <Tabla {turnos} />
    {/if}
</main>

<style>
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
    .view-selector {
        display: flex;
        gap: 1rem;
        margin: 1rem 0;
    }

    main {
        padding: 1rem;
    }
</style>
