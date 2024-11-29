<script>
    let reason = "";
    export let close;
    export let onConfirm;
    let motivos = [
        "Falta de personal para el turno",
        "Falta de insumos para el turno",
        "Otro",
    ];
    function handleConfirm() {
        onConfirm(reason);
    }
</script>

<div class="modal-backdrop" on:click={close}></div>

<div class="modal">
    <div class="modal-content">
        <span class="close" on:click={close}>&times;</span>
        <p>¿Seguro de eliminar el turno?</p>
        <select bind:value={reason}>
            <option value="" disabled selected>Seleccione un motivo</option>
            {#each motivos as motivo}
                <option value={motivo}>{motivo}</option>
            {/each}
        </select>
        <div class="modal-buttons">
            <button on:click={handleConfirm}>Confirmar</button>
        </div>
    </div>
</div>

<style>
    .modal-backdrop {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        z-index: 30; /* Asegurarse de que el backdrop esté por encima del modal principal */
    }

    .modal {
        position: fixed;
        top: 30%; /* Ajustar la posición del modal */
        left: 50%;
        transform: translate(-50%, -30%);
        background: white;
        padding: 1rem;
        border-radius: 8px;
        z-index: 40; /* Asegurarse de que el modal esté por encima del modal principal */
        width: 80%;
        max-width: 500px;
    }

    .modal-content {
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }

    .close {
        position: absolute;
        top: 10px;
        right: 10px;
        font-size: 1.5rem;
        cursor: pointer;
    }

    .modal-buttons {
        display: flex;
        justify-content: center;
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
    select {
        width: 90%; /* Limitar el ancho del cuadro de texto al 90% del modal */
        margin: 0 auto; /* Centrar el cuadro de texto horizontalmente */
        padding: 8px;
        border: 1px solid #ddd;
        border-radius: 4px;
        height: auto; /* Permitir que el select se ajuste a su contenido */
    }
    p {
        font-size: 1.2rem;
        text-align: center;
    }
</style>
