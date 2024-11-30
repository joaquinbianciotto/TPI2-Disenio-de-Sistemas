<script>
    import BajaTurnos from "./bajaTurnos.svelte";
    export let currentEvent;
    export let close;
    export let onDelete;
    let showmodal;

    let editMode = false; 
    let editableEvent = {
        description: '',
        date: '',
        time: '',
        cliente: '',
        dni: '',
        telefono: '',
        email: '',
        marca: '',
        tipo: '',
        patente: ''
    };

    function enableEdit() {
        editMode = true;
        const [date, time] = currentEvent.start.split('T');
        editableEvent = {
            description: currentEvent.description,
            cliente: currentEvent.cliente,
            dni: currentEvent.dni,
            telefono: currentEvent.telefono,
            email: currentEvent.email,
            marca: currentEvent.marca,
            tipo: currentEvent.tipo,
            patente: currentEvent.patente
        }; 
    }

    function saveChanges() {
        currentEvent = {
            ...currentEvent,
            description: editableEvent.description,
            start: `${editableEvent.date}T${editableEvent.time}`,
            cliente: editableEvent.cliente,
            dni: editableEvent.dni
        };
        editMode = false;
        alert("¡Turno modificado con éxito!");
    }

    function cancelEdit() {
        editMode = false;
        editableEvent = {};editMode = false;
        editableEvent = {
            description: '',
            cliente: '',
            dni: '',
            telefono: '',
            email: '',
            marca: '',
            tipo: '',
            patente: '' 
        };
    }


    function eliminarTurno() {
        showmodal = true;
    }
    function closeModal() {
        showmodal = false;
    }
    function borrado(razon) {
        onDelete(razon);
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

    let time = formatDateTime(currentEvent.start).formattedTime;
    let date = formatDateTime(currentEvent.start).formattedDate;

</script>

<div class="modal-backdrop" on:click={close}></div>
<div class="modal">
    <span class="close" on:click={close}>&times;</span>
    <div class="modal-content">
        <h1>{currentEvent.title || "no disponible"}</h1>
        {#if editMode}
                <h2>Editar Datos del Turno</h2>
                <label>
                    <strong>Descripción:</strong>
                    <textarea bind:value={editableEvent.description} class="small-textarea"></textarea>
                </label>

                <label>
                    <strong>Fecha:</strong>
                    <input type="date" bind:value={date} />
                </label>
                <label>
                    <strong>Hora:</strong>
                    <input type="time" bind:value={time} step="3600" min="00:00" max="23:00" pattern="^([01][0-9]|2[0-3]):00$"/>
                </label>

                <h2>Editar Datos del Cliente</h2>
                <label>
                    <strong>Cliente:</strong>
                    <input type="text" bind:value={editableEvent.cliente} />
                </label>
                <label>
                    <strong>DNI:</strong>
                    <input type="text" bind:value={editableEvent.dni} />
                </label>
                <label>
                    <strong>Teléfono:</strong>
                    <input type="text" bind:value={editableEvent.telefono} />
                </label>

                <label>
                    <strong>Email:</strong>
                    <input type="email" bind:value={editableEvent.email} />
                </label>

                <h2>Editar Datos del Vehículo</h2>
                <label>
                    <strong>Marca:</strong>
                    <input type="text" bind:value={editableEvent.marca} />
                </label>
                <label>
                    <strong>Tipo:</strong>
                    <input type="text" bind:value={editableEvent.tipo} />
                </label>
                <label>
                    <strong>Patente:</strong>
                    <input type="text" bind:value={editableEvent.patente} />
                </label>

                <div class="modal-buttons">
                    <button on:click={saveChanges}>Listo</button>
                    <button on:click={cancelEdit}>Cancelar</button>
                </div>
        {:else}
                <h2>Datos del Turno</h2>
                <p>
                    <strong>Descripción:</strong>
                    {currentEvent.description || "No disponible"}
                </p>
                <p><strong>Fecha:</strong> {date || "No disponible"}</p>
                <p><strong>Hora:</strong> {time || "No disponible"}</p>
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
                    <button on:click={enableEdit}>Modificar Turno</button>
                    <button on:click={eliminarTurno}>Eliminar Turno</button>
                </div>
        {/if}
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

    .small-textarea {
        width: 60%; 
        height: 30px; 
        resize: none; 
    }

</style>
