<script>
    import {
        TableHandler,
        Datatable,
        ThSort,
        ThFilter,
    } from "@vincjo/datatables";
    import Modal from "./modal.svelte";
    //Datos de eventos con todos los campos necesarios
    export let turnos = [];
    const table = new TableHandler(turnos, { rowsPerPage: 5 });
    console.log(table.rows);
    let showmodal = false;
    let currentEvent;
    let showMensaje = false;
    let mensaje = "";

    function showModal(row) {
        currentEvent = {
            title: row.title,
            start: row.start,
            description: row.description,
            marca: row.marca,
            tipo: row.tipo,
            categoria: row.categoria,
            cliente: row.cliente,
            dni: row.dni,
            telefono: row.telefono,
            email: row.email,
            patente: row.patente,
        };
        showmodal = true;
        console.log(currentEvent);
    }
    function closeModal() {
        showmodal = false;
        currentEvent = null;
    }
    function formatDateTime(dateTime) {
        const date = new Date(dateTime);
        const formattedDate = date.toLocaleDateString();
        const formattedTime = date.toLocaleTimeString([], {
            hour: "2-digit",
            minute: "2-digit",
        });
        return { formattedDate, formattedTime };
    }
    function handleConfirm() {
        closeModal();
        mensaje = "Turno eliminado exitosamente.";
        showMensaje = true;
        setTimeout(() => {
            showMensaje = false;
        }, 5000);
    }
</script>

<div class="table-container space-y-4">
    <Datatable basic {table}>
        <table>
            <thead>
                <tr class="cabecera">
                    <th><ThSort {table} field="title">Título</ThSort></th>
                    <th><ThSort {table} field="start">Fecha</ThSort></th>
                    <th><ThSort {table} field="start">Hora</ThSort></th>
                    <th><ThSort {table} field="tipo">Tipo</ThSort></th>
                    <th><ThSort {table} field="patente">Patente</ThSort></th>
                    <th><ThSort {table} field="cliente">Cliente</ThSort></th>
                    <th><ThSort {table} field="email">Email</ThSort></th>
                    <th></th>
                </tr>
                <tr>
                    <ThFilter {table} field="title" />
                    <ThFilter {table} field="start" />
                    <ThFilter {table} field="start" />
                    <ThFilter {table} field="tipo" />
                    <ThFilter {table} field="patente" />
                    <ThFilter {table} field="cliente" />

                    <ThFilter {table} field="email" />
                </tr>
            </thead>
            <tbody>
                {#each table.rows as row (row.id)}
                    <tr>
                        <td>{row.title}</td>
                        <td>{formatDateTime(row.start).formattedDate}</td>
                        <td>{formatDateTime(row.start).formattedTime}</td>
                        <td>{row.tipo}</td>
                        <td>{row.patente}</td>
                        <td>{row.cliente}</td>

                        <td>{row.email}</td>
                        <td>
                            <div class="add-button-container">
                                <button on:click={() => showModal(row)}>
                                    Ver detalles
                                </button>
                            </div>
                        </td></tr
                    >
                {/each}
            </tbody>
        </table>
    </Datatable>
</div>
{#if showMensaje}
    <div class="mensaje">{mensaje}</div>
{/if}
<div class="add-button-container">
    <button on:click={() => console.log("agregar")}> Agregar turno </button>
</div>

{#if showmodal && currentEvent}
    <Modal {currentEvent} close={closeModal} onDelete={handleConfirm} />
{/if}

<style>
    .table-container {
        overflow-x: auto;
        background-color: #e0f7fa; /* Color celeste para el contenedor */
        padding: 16px;
        border-radius: 8px;
    }

    table {
        width: 95%;
        border-collapse: collapse;
        border: 1px #ddd solid;
    }

    td {
        padding: 8px;
        text-align: left;
        border-bottom: 2px solid #ddd;
        font-size: 1.1rem;
        border: 1px #ddd solid;
    }
    th {
        padding: 8px;
        text-align: center;
        font-size: 1.2rem;
        text-decoration: none;
        border-bottom: none;
    }
    tr {
        font-size: 1.2rem;
    }
    .cabecera {
        background-color: #0288d1; /* Color azul para la cabecera */
        font-size: 1.2rem;
    }
    thead {
        background-color: #0288d1; /* Color azul para la cabecera */
        font-size: 1;
    }

    @media (max-width: 768px) {
        th:nth-child(3), td:nth-child(3), /* Marca */
        th:nth-child(4), td:nth-child(4), /* Tipo */
        th:nth-child(5), td:nth-child(5), /* Categoría */
        th:nth-child(6), td:nth-child(6), /* Cliente */
        th:nth-child(7), td:nth-child(7), /* DNI */
        th:nth-child(8), td:nth-child(8), /* Teléfono */
        th:nth-child(9), td:nth-child(9)  /* Email */ {
            display: none;
        }
    }
    .add-button-container {
        display: flex;
        justify-content: flex-end;
        margin-top: 16px;
    }

    .add-button-container button {
        background-color: #0288d1;
        color: white;
        padding: 8px 16px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }

    .add-button-container button:hover {
        background-color: #0277bd;
    }
    .mensaje {
        background-color: #dff0d8;
        color: #3c763d;
        padding: 10px;
        border-radius: 4px;
        margin-bottom: 16px;
        text-align: center;
        font-size: 1.2rem;
    }
</style>
