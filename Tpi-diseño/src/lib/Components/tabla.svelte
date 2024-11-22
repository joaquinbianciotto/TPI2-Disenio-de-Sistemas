<script>
    // @ts-nocheck

    import { onMount } from "svelte";
    import { Calendar } from "@fullcalendar/core";
    import { tick } from "svelte";
    import dayGridPlugin from "@fullcalendar/daygrid";
    import timeGridPlugin from "@fullcalendar/timegrid";

    let calendarElement;
    let calendar;
    let showModal = false;
    let currentEvent = null;

    export let turnos;
    //Datos de eventos con todos los campos necesarios
    let events = [
        {
            id: "1",
            title: "Revisión SUV",
            start: "2024-08-27T10:00:00",
            description: "Revisión técnica para SUV.",
            marca: "Toyota",
            tipo: "SUV",
            categoria: "B",
            cliente: "Juan Pérez",
            dni: "12345678",
            telefono: "123456789",
            email: "juanperez@example.com",
        },
        {
            id: "2",
            title: "Revisión Sedán",
            start: "2024-08-28T14:00:00",
            description: "Revisión técnica para sedán.",
            marca: "Honda",
            tipo: "Sedán",
            categoria: "A",
            cliente: "María Gómez",
            dni: "23456789",
            telefono: "234567890",
            email: "mariagomez@example.com",
        },
    ];

    let event = turnos?.map((turno) => ({
        id: turno.id,
        title: `Revisión ${turno.vehiculo.tipo}`,
        start: turno.fecha,
        description: `Revisión técnica para ${turno.vehiculo.tipo}.`,
        marca: turno.vehiculo.marca,
        tipo: turno.vehiculo.tipo,
        categoria: turno.vehiculo.categoria,
        cliente: turno.cliente.nombre,
        dni: turno.cliente.cuit,
        telefono: turno.cliente.telefono,
        email: turno.cliente.email,
    }));

    // Estados para los filtros
    let tipoFiltro = "";
    let marcaFiltro = "";
    let categoriaFiltro = "";

    //estado para busqueda
    let busqueda = "";

    let mostrarCalendario = true;

    const searchFields = [
        "title",
        "description",
        "marca",
        "tipo",
        "categoria",
        "cliente",
        "dni",
        "telefono",
        "email",
    ];
    // Filtros únicos para los selectores
    let tipos = [...new Set(events.map((event) => event.tipo))];
    let marcas = [...new Set(events.map((event) => event.marca))];
    let categorias = [...new Set(events.map((event) => event.categoria))];

    // Función para aplicar filtros
    function aplicarFiltros() {
        let filteredEvents = events.filter((event) => {
            return (
                (!tipoFiltro || event.tipo === tipoFiltro) &&
                (!marcaFiltro || event.marca === marcaFiltro) &&
                (!categoriaFiltro || event.categoria === categoriaFiltro)
            );
        });
        calendar.getEvents().forEach((e) => e.remove());
        calendar.addEventSource(filteredEvents);
    }
    //funcion para buscar
    function buscar() {
        // Filtrar eventos por busqueda
        let filteredEvents = events.filter((event) => {
            // Buscar en los campos definidos en searchFields
            return (
                !busqueda ||
                // Buscar en cada campo del evento si alguno incluye el parametro de busqueda
                searchFields.some((field) => {
                    if (field in event) {
                        return event[field]
                            .toLowerCase()
                            .includes(busqueda.toLowerCase());
                    }
                    return false;
                })
            );
        });
        if (mostrarCalendario) {
            calendar.getEvents().forEach((e) => e.remove());
            calendar.addEventSource(filteredEvents);
        }
    }
    //funcion para cambiar vista entre calendario y lista
    async function toggleVista() {
        mostrarCalendario = !mostrarCalendario;
        if (mostrarCalendario) {
            await tick(); // Esperar a que el DOM se actualice
            initializeCalendar();
        }
    }
    // Inicializar el calendario
    function initializeCalendar() {
        if (calendar) {
            calendar.destroy();
        }
        calendar = new Calendar(calendarElement, {
            plugins: [dayGridPlugin, timeGridPlugin],
            initialView: "timeGridWeek",
            events: events,
            eventClick: function (info) {
                currentEvent = {
                    title: info.event.title,
                    start: info.event.startStr,
                    description: info.event.extendedProps.description,
                    marca: info.event.extendedProps.marca,
                    tipo: info.event.extendedProps.tipo,
                    categoria: info.event.extendedProps.categoria,
                    cliente: info.event.extendedProps.cliente,
                    dni: info.event.extendedProps.dni,
                    telefono: info.event.extendedProps.telefono,
                    email: info.event.extendedProps.email,
                };
                showModal = true;
            },
        });

        calendar.render();
    }
    onMount(() => {
        if (mostrarCalendario) {
            initializeCalendar();
        }
    });

    function closeModal() {
        showModal = false;
        currentEvent = null;
    }
</script>

<div>
    <label for="tipo">Tipo:</label>
    <select id="tipo" bind:value={tipoFiltro} on:change={aplicarFiltros}>
        <option value="">Todos</option>
        {#each tipos as tipo}
            <option value={tipo}>{tipo}</option>
        {/each}
    </select>

    <label for="marca">Marca:</label>
    <select id="marca" bind:value={marcaFiltro} on:change={aplicarFiltros}>
        <option value="">Todas</option>
        {#each marcas as marca}
            <option value={marca}>{marca}</option>
        {/each}
    </select>

    <label for="categoria">Categoría:</label>
    <select
        id="categoria"
        bind:value={categoriaFiltro}
        on:change={aplicarFiltros}
    >
        <option value="">Todas</option>
        {#each categorias as categoria}
            <option value={categoria}>{categoria}</option>
        {/each}
    </select>
    <input
        type="text"
        id="busqueda"
        bind:value={busqueda}
        placeholder="Buscar turnos..."
    />
    <button on:click={buscar}>Buscar</button>
    <button on:click={toggleVista}>
        {#if mostrarCalendario}
            Ver como Lista
        {:else}
            Ver como Calendario
        {/if}
    </button>
</div>

<div bind:this={calendarElement} id="calendar"></div>

{#if mostrarCalendario}
    <div bind:this={calendarElement} id="calendar"></div>
{:else}
    <ListaTurnos {events} {busqueda} {searchFields} />
{/if}

{#if showModal && currentEvent}
    <Modal {currentEvent} close={closeModal} />
{/if}

<style>
    #calendar {
        max-width: 900px;
        margin: 0 auto;
    }

    div {
        margin-bottom: 20px;
    }

    select {
        margin-right: 10px;
    }
</style>
