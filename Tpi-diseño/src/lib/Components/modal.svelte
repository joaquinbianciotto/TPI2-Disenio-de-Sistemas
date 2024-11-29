<script>
    import BajaTurnos from "./bajaTurnos.svelte";
    export let currentEvent;
    export let close;

    let showmodal;
    function eliminarTurno() {
        showmodal = true;
    }
    function closeModal() {
        showmodal = false;
    }
</script>

<div class="modal-backdrop" on:click={close}></div>
<div class="modal">
    <div class="modal-content">
        <span class="close" on:click={close}>&times;</span>

        <h1>{currentEvent.title || "no disponible"}</h1>

        <h2>Datos del Turno</h2>
        <p>
            <strong>Descripción:</strong>
            {currentEvent.description || "No disponible"}
        </p>
        <p><strong>Fecha:</strong> {currentEvent.start || "No disponible"}</p>

        <h2>Datos del Cliente</h2>
        <p>
            <strong>Cliente:</strong>
            {currentEvent.cliente || "No disponible"}
        </p>
        <p><strong>DNI:</strong> {currentEvent.dni || "No disponible"}</p>
        <p>
            <strong>Teléfono:</strong>
            {currentEvent.telefono || "No disponible"}
        </p>
        <p><strong>Email:</strong> {currentEvent.email || "No disponible"}</p>

        <h2>Datos del Vehículo</h2>
        <p><strong>Marca:</strong> {currentEvent.marca || "No disponible"}</p>
        <p><strong>Tipo:</strong> {currentEvent.tipo || "No disponible"}</p>
        <p>
            <strong>Patente:</strong>
            {currentEvent.patente || "No disponible"}
        </p>

        <div class="modal-buttons">
            <button on:click={console.log("mod")}>Modificar Turno</button>
            <button on:click={eliminarTurno}>Eliminar Turno</button>
        </div>
    </div>
</div>
{#if showmodal}
    <BajaTurnos close={closeModal} />
{/if}

<style>
    .modal-backdrop {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        z-index: 10;
    }
    .modal {
        position: fixed;
        top: 20%; /* Ajustar la posición del modal más arriba */
        left: 50%;
        transform: translate(-50%, -20%);
        background: white;
        padding: 1rem;
        border-radius: 8px;
        z-index: 20;
        width: 80%;
        max-width: 500px;
    }

    .modal-content {
        background-color: #fefefe;
        margin-top: 10% auto;
        padding: 20px;
        border: 1px solid #888;
        width: 90%;
        max-width: 600px;
    }

    .close {
        color: #aaa;
        float: right;
        font-size: 28px;
        font-weight: bold;
    }

    .close:hover,
    .close:focus {
        color: black;
        text-decoration: none;
        cursor: pointer;
    }

    .modal-buttons {
        display: flex;
        justify-content: space-between;
        margin-top: 20px;
    }

    .modal-buttons button {
        background-color: #0288d1;
        color: white;
        padding: 8px 16px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }
</style>
