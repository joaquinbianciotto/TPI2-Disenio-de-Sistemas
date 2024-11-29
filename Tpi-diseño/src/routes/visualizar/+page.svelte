<script>
    import { goto } from "$app/navigation";
    import { onMount } from "svelte";
    import Calendario from "$lib/Components/calendario.svelte";
    import Lista from "$lib/Components/lista.svelte";
    import Tabla from "$lib/Components/tabla.svelte";
    let username = "Usuario";
    let turnos = [
        {
            id: "1",
            title: "Revisión SUV",
            start: "2024-11-27T10:00:00",
            description: "Revisión técnica para SUV.",
            marca: "Toyota",
            tipo: "SUV",
            categoria: "B",
            cliente: "Juan Pérez",
            dni: "12345678",
            telefono: "123456789",
            email: "juanperez@example.com",
            patente: "ABC123",
        },
        {
            id: "2",
            title: "Revisión Sedán",
            start: "2024-11-22T14:00:00",
            description: "Revisión técnica para sedán.",
            marca: "Honda",
            tipo: "Sedán",
            categoria: "A",
            cliente: "María Gómez",
            dni: "23456789",
            telefono: "234567890",
            email: "mariagomez@example.com",
            patente: "DEF456",
        },
        {
            id: "3",
            title: "Revisión Hatchback",
            start: "2024-11-23T09:00:00",
            description: "Revisión técnica para hatchback.",
            marca: "Ford",
            tipo: "Hatchback",
            categoria: "C",
            cliente: "Carlos López",
            dni: "34567890",
            telefono: "345678901",
            email: "carloslopez@example.com",
            patente: "GHI789",
        },
        {
            id: "4",
            title: "Revisión Moto",
            start: "2024-11-24T11:00:00",
            description: "Revisión técnica para moto.",
            marca: "Yamaha",
            tipo: "Moto",
            categoria: "D",
            cliente: "Ana Martínez",
            dni: "45678901",
            telefono: "456789012",
            email: "anamartinez@example.com",
            patente: "JKL012",
        },
        {
            id: "5",
            title: "Revisión Coupe",
            start: "2024-11-25T13:00:00",
            description: "Revisión técnica para coupe.",
            marca: "BMW",
            tipo: "Coupe",
            categoria: "E",
            cliente: "Luis Fernández",
            dni: "56789012",
            telefono: "567890123",
            email: "luisfernandez@example.com",
            patente: "MNO345",
        },
        {
            id: "6",
            title: "Revisión SUV",
            start: "2024-11-26T15:00:00",
            description: "Revisión técnica para SUV.",
            marca: "BMW",
            tipo: "SUV",
            categoria: "B",
            cliente: "Laura García",
            dni: "67890123",
            telefono: "678901234",
            email: "lauragarcia@example.com",
            patente: "PQR678",
        },
        {
            id: "7",
            title: "Revisión Sedán",
            start: "2024-11-27T17:00:00",
            description: "Revisión técnica para sedán.",
            marca: "Audi",
            tipo: "Sedán",
            categoria: "A",
            cliente: "Pedro Sánchez",
            dni: "78901234",
            telefono: "789012345",
            email: "pedrosanchez@example.com",
            patente: "STU901",
        },
        {
            id: "8",
            title: "Cambio de frenos",
            start: "2024-11-28T08:00:00",
            description: "Cambio de frenos delanteros y traseros.",
            marca: "Chevrolet",
            tipo: "Camioneta",
            categoria: "C",
            cliente: "Marta Rodríguez",
            dni: "89012345",
            telefono: "890123456",
            email: "martarodriguez@example.com",
            patente: "VWX234",
        },
        {
            id: "9",
            title: "Revisión Moto",
            start: "2024-11-29T10:00:00",
            description: "Revisión técnica para moto.",
            marca: "Suzuki",
            tipo: "Moto",
            categoria: "D",
            cliente: "Jorge Ramírez",
            dni: "90123456",
            telefono: "901234567",
            email: "jorgeramirez@example.com",
            patente: "YZA567",
        },
        {
            id: "10",
            title: "Revisión Camión",
            start: "2024-11-30T12:00:00",
            description: "Revisión técnica para camión.",
            marca: "Volvo",
            tipo: "Camión",
            categoria: "E",
            cliente: "Sofía Torres",
            dni: "01234567",
            telefono: "012345678",
            email: "sofiatorres@example.com",
            patente: "BCD890",
        },
        {
            id: "11",
            title: "Revisión Crossover",
            start: "2024-12-01T14:00:00",
            description: "Revisión técnica para crossover.",
            marca: "Nissan",
            tipo: "Crossover",
            categoria: "K",
            cliente: "Diego Ramírez",
            dni: "12345679",
            telefono: "123456790",
            email: "diegoramirez@example.com",
            patente: "EFG123",
        },
        {
            id: "12",
            title: "Revisión Microcar",
            start: "2024-12-02T16:00:00",
            description: "Revisión técnica para microcar.",
            marca: "Smart",
            tipo: "Microcar",
            categoria: "L",
            cliente: "Elena Moreno",
            dni: "23456780",
            telefono: "234567801",
            email: "elenamoreno@example.com",
            patente: "HIJ456",
        },
        {
            id: "13",
            title: "Revisión Microcar",
            start: "2024-12-02T16:00:00",
            description: "Revisión técnica para Sedan.",
            marca: "Chevrolet",
            tipo: "Sedán",
            categoria: "L",
            cliente: "Joaquin Bianciotto",
            dni: "87654321",
            telefono: "234567801",
            email: "joaquinbianciotto@example.com",
            patente: "LDT999",
        },
    ];

    // Recuperar el usuario desde localStorage al cargar la página
    onMount(() => {
        username = localStorage.getItem("loggedInUser") || "Usuario";
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
        background-color: blue;
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
