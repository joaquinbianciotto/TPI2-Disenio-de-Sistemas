<script>
    let reason = "";
    export let close;
</script>

<div class="modal-backdrop" on:click={close}></div>

<div class="modal">
    <div class="modal-content">
        <span class="close" on:click={close}>&times;</span>
        <p>¿Seguro de eliminar el turno?</p>
        <textarea bind:value={reason} placeholder="Ingrese el motivo"
        ></textarea>
        <button on:click={close}>Confirmar</button>
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
