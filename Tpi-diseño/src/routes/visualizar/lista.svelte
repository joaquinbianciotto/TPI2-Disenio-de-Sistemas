<script>
    export let busqueda = "";
    export let searchFields = [
        "id",
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
    export let events = [
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
    ];

    function buscar() {
        return events.filter((events) => {
            return searchFields.some((field) => {
                return events[field]
                    .toLowerCase()
                    .includes(busqueda.toLowerCase());
            });
        });
    }
</script>

<div>
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
