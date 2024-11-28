<script>
    export let currentEvent;
    export let close;

    console.log(currentEvent);
</script>

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
            <button on:click={console.log("borrar")}>Eliminar Turno</button>
        </div>
    </div>
</div>

<style>
    .modal {
        display: block;
        position: fixed;
        z-index: 1;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        overflow: auto;
        background-color: rgba(0, 0, 0, 0.4);
    }

    .modal-content {
        background-color: #fefefe;
        margin: 15% auto;
        padding: 20px;
        border: 1px solid #888;
        width: 80%;
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
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
    }
</style>
