<script>
    import {
        TableHandler,
        Datatable,
        ThSort,
        ThFilter,
    } from "@vincjo/datatables";
    //Datos de eventos con todos los campos necesarios
    export let turnos = [];
    const table = new TableHandler(turnos, { rowsPerPage: 5 });
    const search = table.createSearch();
</script>

<div class="table-container space-y-4">
    <Datatable basic {table}>
        <table>
            <thead>
                <tr>
                    <ThSort {table} field="title">Título</ThSort>
                    <ThSort {table} field="start">Fecha</ThSort>
                    <ThSort {table} field="tipo">Tipo</ThSort>
                    <ThSort {table} field="patente">Patente</ThSort>
                    <ThSort {table} field="cliente">Cliente</ThSort>
                    <ThSort {table} field="email">Email</ThSort>
                </tr>
                <tr>
                    <ThFilter {table} field="title" />
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
                        <td>{row.start}</td>

                        <td>{row.tipo}</td>
                        <td>{row.patente}</td>
                        <td>{row.cliente}</td>

                        <td>{row.email}</td>
                        <td>
                            <button on:click={() => console.log(row.cliente)}>
                                ver mas
                            </button>
                        </td></tr
                    >
                {/each}
            </tbody>
        </table>
    </Datatable>
</div>
<div class="add-button-container">
    <button on:click={() => console.log("Agregar elemento")}>
        Agregar elemento
    </button>
</div>

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
    }

    th,
    td {
        padding: 8px;
        text-align: left;
        border-bottom: 2px solid #ddd;
    }

    thead {
        background-color: #0288d1; /* Color azul para la cabecera */
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
</style>
