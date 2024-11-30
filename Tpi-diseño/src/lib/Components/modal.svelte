<script>
    import BajaTurnos from "./bajaTurnos.svelte";
    export let currentEvent;
    export let close;
    export let onDelete;
    let showmodal;
    function eliminarTurno() {
        showmodal = true;
    }
    function closeModal() {
        showmodal = false;
    }
    function borrado(razon) {
        onDelete(razon);
    }
</script>

<div class="modal-backdrop" on:click={close}></div>
<div class="modal">
    <span class="close" on:click={close}>&times;</span>
    <div class="modal-content">
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
    <BajaTurnos close={closeModal} onConfirm={borrado} />
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
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: white;
        padding: 1rem;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        border-radius: 8px;
        z-index: 1000;
        width: 90%;
        max-width: 30%;
        max-height: 77%; /* Ajusta la altura máxima del modal */
        overflow-y: auto; /* Permite el desplazamiento si el contenido es demasiado alto */
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .modal-content {
        background-color: #fefefe;
        margin-top: 10%;
        padding: 20px;
        border: 1px solid #888;
        width: 90%;
        max-width: 600px;
        justify-content: center;
    }

    .close {
        color: #aaa;
        float: right;
        font-size: 28px;
        font-weight: bold;
        position: absolute;
        right: 10px;
        top: 10px;
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
