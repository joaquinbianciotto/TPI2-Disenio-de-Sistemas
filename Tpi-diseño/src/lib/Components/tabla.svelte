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
    const rows = table;
</script>

<div class="table-container space-y-4">
    <Datatable basic {table}>
        <table>
            <thead>
                <tr>
                    <ThSort {table} field="title">Título</ThSort>
                    <ThSort {table} field="start">Fecha</ThSort>
                    <ThSort {table} field="marca">Marca</ThSort>
                    <ThSort {table} field="tipo">Tipo</ThSort>
                    <ThSort {table} field="categoria">Categoría</ThSort>
                    <ThSort {table} field="cliente">Cliente</ThSort>
                    <ThSort {table} field="dni">DNI</ThSort>
                    <ThSort {table} field="telefono">Teléfono</ThSort>
                    <ThSort {table} field="email">Email</ThSort>
                </tr>
                <tr>
                    <ThFilter {table} field="title" />
                    <ThFilter {table} field="start" />
                    <ThFilter {table} field="marca" />
                    <ThFilter {table} field="tipo" />
                    <ThFilter {table} field="categoria" />
                    <ThFilter {table} field="cliente" />
                    <ThFilter {table} field="dni" />
                    <ThFilter {table} field="telefono" />
                    <ThFilter {table} field="email" />
                </tr>
            </thead>
            <tbody>
                {#each table.rows as row (row.id)}
                    <tr>
                        <td>{row.title}</td>
                        <td>{row.start}</td>
                        <td>{row.marca}</td>
                        <td>{row.tipo}</td>
                        <td>{row.categoria}</td>
                        <td>{row.cliente}</td>
                        <td>{row.dni}</td>
                        <td>{row.telefono}</td>
                        <td>{row.email}</td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </Datatable>
</div>
