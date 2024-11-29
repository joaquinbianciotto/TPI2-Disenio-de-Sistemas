<script>
    /**
     * @type {any[]}
     */

    let busqueda = "";

    let searchFields = [
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

    function buscar() {
        // Filtrar eventos por busqueda
        let filteredEvents = turnos.filter((event) => {
            // Si no hay búsqueda, devolver todos los eventos
            if (!busqueda) return true;

            // Convertir la búsqueda a minúsculas
            const lowerCaseBusqueda = busqueda.toLowerCase();

            // Buscar en cada campo del evento si alguno incluye el parámetro de búsqueda
            return searchFields.some(
                (field) =>
                    field in event &&
                    event[field].toLowerCase().includes(lowerCaseBusqueda),
            );
        });

        return filteredEvents;
    }

    //Datos de eventos con todos los campos necesarios
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
        },
    ];
</script>

<div>
    {#if turnos.length === 0}
        <p>No hay turnos disponibles.</p>
    {:else}
        {#each buscar() as event}
            <div class="turno">
                <h3>{event.title}</h3>
                <p>{event.description}</p>
                <div class="container">
                    <div class="column">
                        <p><strong>Marca:</strong> {event.marca}</p>
                        <p><strong>Tipo:</strong> {event.tipo}</p>
                        <p><strong>Categoría:</strong> {event.categoria}</p>
                    </div>
                    <div class="column">
                        <p>
                            <strong>Fecha:</strong>
                            {new Date(event.start).toLocaleDateString()}
                        </p>
                        <p>
                            <strong>Hora:</strong>
                            {new Date(event.start).toLocaleTimeString()}
                        </p>
                    </div>
                    <div class="column">
                        <p><strong>Cliente:</strong> {event.cliente}</p>
                        <p><strong>DNI:</strong> {event.dni}</p>
                        <p><strong>Teléfono:</strong> {event.telefono}</p>
                        <p><strong>Email:</strong> {event.email}</p>
                    </div>
                </div>
            </div>
        {/each}
    {/if}
</div>

<style>
    .turno {
        border: 1px solid #ccc;
        padding: 10px;
        margin: 10px 0;
    }
    .container {
        display: flex;
        justify-content: space-between;
    }
    .column {
        flex: 1;
        padding: 10px;
    }
</style>
