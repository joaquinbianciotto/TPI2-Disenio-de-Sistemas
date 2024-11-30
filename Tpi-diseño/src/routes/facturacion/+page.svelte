<script lang="ts">
    import { onMount } from "svelte";
    import rtos from "../../patentes.js";

    let username = "Usuario";

    onMount(() => {
        username = localStorage.getItem("loggedInUser") || "Usuario";
    });

    let patenteSeleccionada = "";
    let cliente = { nombre: "", dni: "", telefono: "", email: "" };
    let vehiculo = { marca: "", modelo: "", año: "", tipo: "" };
    let metodoPago = "";
    let facturaGenerada = false;
    let fecha ='';
    let tarjetaNumero = "";
    let tarjetaTitular = "";
    let tarjetaVencimiento = "";
    let tarjetaCvv = "";
    let monto = 0;

    function actualizarDetallesVehiculo() {
        const vehiculoSeleccionado = rtos.find(
            (veh) => veh.patente === patenteSeleccionada,
        );
        if (vehiculoSeleccionado) {
            vehiculo.marca = vehiculoSeleccionado.marca;
            vehiculo.modelo = vehiculoSeleccionado.modelo;
            vehiculo.año = vehiculoSeleccionado.año;
            vehiculo.tipo = vehiculoSeleccionado.tipo;
            monto = obtenerMontoPorTipoVehiculo(vehiculo.tipo);
        } else {
            vehiculo.marca = "";
            vehiculo.modelo = "";
            vehiculo.año = "";
            vehiculo.tipo = "";
        }
    }

    function generarFactura() {
        if (metodoPago === "Debito" && (!tarjetaNumero || !tarjetaTitular)) {
            alert("Por favor, complete los datos de la tarjeta.");
            return;
        }
        facturaGenerada = true;
    }

    function obtenerMontoPorTipoVehiculo(tipo: string): number {
        switch (tipo) {
            case "automovil":
                return 500;
            case "trailer":
                return 1000;
            case "camion":
                return 2500;
            case "autobus":
                return 2000;
            default:
                return 0;
        }
    }

    function descargarFactura() {
        const contenido = `
        Factura Generada:
        Fecha: ${fecha}
        Cliente: ${cliente.nombre}
        DNI: ${cliente.dni}
        Teléfono: ${cliente.telefono}

        Vehículo:
        Patente: ${patenteSeleccionada}
        Marca: ${vehiculo.marca}
        Modelo: ${vehiculo.modelo}
        Año: ${vehiculo.año}
        Tipo: ${vehiculo.tipo}

        Método de Pago: ${metodoPago}
        Monto: $${monto}`;

        const blob = new Blob([contenido], { type: "text/plain" });
        const enlace = document.createElement("a");
        enlace.href = URL.createObjectURL(blob);
        enlace.download = `Factura_${patenteSeleccionada}.txt`;
        enlace.click();
    }

    function formatFechaVencimiento(event: Event) {
        const input = event.target as HTMLInputElement | null;
        if (input && input.value) {
            let value = input.value.replace(/\D/g, "");
            if (value.length >= 3) {
                value = value.slice(0, 2) + "/" + value.slice(2, 4);
            }
            fecha = value;
        }
    }

    function formatearNumeroTarjeta(event: Event) {
        const target = event.target as HTMLInputElement | null;
        if (!target) return;
        let input = target.value.replace(/\D/g, "");
        if (input.length > 16) {
            input = input.slice(0, 16);
        }
        input = input.match(/.{1,4}/g)?.join("-") || input;
        tarjetaNumero = input;
    }

    function formatearDNI(event: Event) {
        const target = event.target as HTMLInputElement | null;
        if (!target) return;
        let input = target.value.replace(/\D/g, "");
        if (input.length > 8) {
            input = input.slice(0, 8);
        }
        if (input.length > 5) {
            input =
                input.slice(0, 2) +
                "." +
                input.slice(2, 5) +
                "." +
                input.slice(5);
        } else if (input.length > 2) {
            input = input.slice(0, 2) + "." + input.slice(2);
        }
        cliente.dni = input;
    }
</script>

{#if !facturaGenerada}
    <div class="form-container">
        <h2>Generar Factura</h2>

        <form on:submit|preventDefault={generarFactura}>
            <!-- Selección de Patente -->
            <div class="form-group">
                <label for="patente">Seleccionar Patente:</label>
                <select
                    id="patente"
                    bind:value={patenteSeleccionada}
                    on:change={actualizarDetallesVehiculo}
                    required
                >
                    <option value="">Seleccione una patente</option>
                    {#each rtos as rto}
                        <option value={rto.patente}
                            >{rto.patente} - {rto.marca} {rto.modelo}</option
                        >
                    {/each}
                </select>
            </div>

            <!-- Datos del Cliente -->
            <h3>Datos del Cliente</h3>
            <div class="form-row">
                <div class="form-group">
                    <label for="clienteNombre">Nombre:</label>
                    <input
                        type="text"
                        id="clienteNombre"
                        bind:value={cliente.nombre}
                        placeholder="Nombre del cliente"
                        required
                    />
                </div>
                <div class="form-group">
                    <label for="clienteDni">DNI:</label>
                    <input
                        type="text"
                        id="clienteDni"
                        bind:value={cliente.dni}
                        placeholder="DNI del cliente"
                        maxlength="11"
                        on:input={formatearDNI}
                        required
                    />
                </div>
                <div class="form-group">
                    <label for="clienteTelefono">Teléfono:</label>
                    <input
                        type="tel"
                        id="clienteTelefono"
                        bind:value={cliente.telefono}
                        placeholder="Teléfono del cliente"
                        maxlength="10"
                        required
                    />
                </div>
            </div>

            <!-- Detalles del Vehículo -->
            <h3>Detalles del Vehículo</h3>
            <div class="form-row">
                <div class="form-group">
                    <label for="vehiculoMarca">Marca:</label>
                    <input
                        type="text"
                        id="vehiculoMarca"
                        bind:value={vehiculo.marca}
                        readonly
                    />
                </div>
                <div class="form-group">
                    <label for="vehiculoModelo">Modelo:</label>
                    <input
                        type="text"
                        id="vehiculoModelo"
                        bind:value={vehiculo.modelo}
                        readonly
                    />
                </div>

                <div>
                    <label for="vehiculoAño">Año:</label>
                    <input
                        type="text"
                        id="vehiculoAño"
                        bind:value={vehiculo.año}
                        readonly
                    />
                </div>

                <div>
                    <label for="vehiculoTipo">Tipo:</label>
                    <input
                        type="text"
                        id="vehiculoTipo"
                        bind:value={vehiculo.tipo}
                        readonly
                    />
                </div>
            </div>

            <!-- Método de Pago -->
            <h3>Método de Pago</h3>
            <div class="form-group">
                <label for="metodoPago">Seleccione un método:</label>
                <select id="metodoPago" bind:value={metodoPago} required>
                    <option value="">Seleccione un método de pago</option>
                    <option value="Efectivo">Efectivo</option>
                    <option value="Debito">Debito</option>
                </select>
            </div>

            {#if metodoPago === "Debito"}
                <p>Datos de tarjeta</p>
                <div class="form-row">
                    <div class="form-group">
                        <label for="tarjetaNumero">Número de tarjeta:</label>
                        <input
                            type="text"
                            id="tarjetaNumero"
                            placeholder="Número de tarjeta"
                            bind:value={tarjetaNumero}
                            on:input={formatearNumeroTarjeta}
                            required
                        />
                    </div>
                    <div class="form-group">
                        <label for="tarjetaTitular">Titular de la Tarjeta:</label>
                        <input
                            type="text"
                            id="tarjetaTitular"
                            bind:value={tarjetaTitular}
                            placeholder="Titular de la tarjeta"
                            required
                        />
                    </div>
                    <div class="form-group">
                        <label for="tarjetaVencimiento"
                            >Fecha de vencimiento:</label
                        >
                        <input
                            type="text"
                            id="tarjetaVencimiento"
                            placeholder="MM/AA"
                            maxlength="5"
                            bind:value={fecha}
                            on:input={formatFechaVencimiento}
                            required
                        />
                    </div>
                </div>
            {/if}

            <h3>Monto: {monto}</h3>
            <button type="submit">Confirmar Pago</button>
        </form>
    </div>
{:else}
    <div class="resumen-container">
        <h2>Resumen de Factura</h2>
        <p>Cliente: {cliente.nombre}</p>
        <p>DNI: {cliente.dni}</p>
        <p>Teléfono: {cliente.telefono}</p>

        <p>Vehículo:</p>
        <ul>
            <li>Patente: {patenteSeleccionada}</li>
            <li>Marca: {vehiculo.marca}</li>
            <li>Modelo: {vehiculo.modelo}</li>
            <li>Año: {vehiculo.año}</li>
            <li>Tipo: {vehiculo.tipo}</li>
        </ul>

        <p>Método de Pago: {metodoPago}</p>
        <p>Monto: ${monto}</p>

        <button on:click={descargarFactura}>Descargar Factura</button>
        <br />
        <button on:click={() => (facturaGenerada = false)}>Generar otra factura</button>
    </div>
{/if}

<style>
    .form-container {
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
        background-color: #f9f9f9;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    .form-row {
        display: flex;
        flex-wrap: wrap;
        gap: 20px;
    }

    .form-group {
        flex: 1 1 calc(50% - 20px);
        display: flex;
        flex-direction: column;
    }

    label {
        font-weight: bold;
        margin-bottom: 5px;
    }

    input,
    select {
        padding: 10px;
        font-size: 1rem;
        border: 1px solid #ccc;
        border-radius: 5px;
        width: 100%;
    }

    button {
        margin-top: 20px;
        padding: 10px 20px;
        background-color: #007bff;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }

    button:hover {
        background-color: #007bff;
    }

    h3 {
        margin-top: 20px;
        margin-bottom: 10px;
    }

    .resumen-container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        text-align: center;
        height: 100vh; /* Asegura que ocupe toda la altura de la pantalla */
        margin: 0; /* Elimina márgenes extra */
    }

    .resumen-container h2 {
        margin-bottom: 20px;
    }

    .resumen-container p {
        margin: 10px 0;
    }

    .resumen-container ul {
        list-style: none;
        padding: 0;
    }

    .resumen-container ul li {
        margin: 5px 0;
    }

    .resumen-container button {
        margin-top: 20px;
        padding: 10px 20px;
        background-color: #007bff;
        color: #fff;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-size: 16px;
    }

    .resumen-container button:hover {
        background-color: #0056b3;
    }
</style>
