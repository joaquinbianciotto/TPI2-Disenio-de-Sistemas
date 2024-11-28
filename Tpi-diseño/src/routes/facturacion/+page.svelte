<script lang="ts"> 
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import  rtos  from '../../patentes.js';

    

  let username = 'Usuario'; 

  // Recuperar el usuario desde localStorage al cargar la página
  onMount(() => {
    username = localStorage.getItem('loggedInUser') || 'Usuario';
  });
    

    let patenteSeleccionada = '';
    let cliente = { nombre: '', dni: '', telefono: '', email: '' };  // Datos del cliente
    let vehiculo = { marca: '', modelo: '', año: '', tipo:''};  // Datos del vehículo
    let metodoPago = '';
    let facturaGenerada = false;
    let fecha = '';
    let tarjetaNumero = '';
    let dni = '';
    let monto = 0;

    function actualizarDetallesVehiculo() {
        const vehiculoSeleccionado = rtos.find(veh => veh.patente === patenteSeleccionada);
        if (vehiculoSeleccionado) {
            vehiculo.marca = vehiculoSeleccionado.marca;
            vehiculo.modelo = vehiculoSeleccionado.modelo;
            vehiculo.año = vehiculoSeleccionado.año;
            vehiculo.tipo = vehiculoSeleccionado.tipo;
            monto = obtenerMontoPorTipoVehiculo(vehiculo.tipo);
        } else {
            vehiculo.marca = '';
            vehiculo.modelo = '';
            vehiculo.año = '';
            vehiculo.tipo = '';
        }
    }

    
    function generarFactura() {
        if (patenteSeleccionada) {
            actualizarDetallesVehiculo();
            facturaGenerada = true;
        }
    }

    function descargarComprobante() {
        const facturaTexto = `
            Factura Generada:
            -------------------
            Patente: ${patenteSeleccionada}
            Marca: ${vehiculo.marca}
            Modelo: ${vehiculo.modelo}
            Año: ${vehiculo.año}

            Cliente:
            --------
            Nombre: ${cliente.nombre}
            DNI: ${cliente.dni}
            Teléfono: ${cliente.telefono}
            
            Monto: ${monto}

            Método de Pago: ${metodoPago}
        `;

        const blob = new Blob([facturaTexto], { type: 'text/plain' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'comprobante_factura.txt';
        a.click();
        URL.revokeObjectURL(url);  
    }

    function obtenerMontoPorTipoVehiculo(tipo: string): number {
        switch (tipo) {
            case 'automovil':
                return 500;
            case 'trailer':
                return 1000;
            case 'camion':
                return 2500;
            case 'autobus':
                return 2000;    
            default:
                return 0;
        }
    }


    // funciones de controladores de imputs

    function formatFechaVencimiento(event : Event ) {
        const input = event.target as HTMLInputElement | null;
        if (input && input.value) {
            let value = input.value.replace(/\D/g, ''); 
            if (value.length >= 3) {
                value = value.slice(0, 2) + '/' + value.slice(2, 4); 
            }
            fecha = value;
        }
    }

    function formatearNumeroTarjeta(event: Event) {
        const target = event.target as HTMLInputElement | null;
        if (!target) return; 
            let input = target.value.replace(/\D/g, ''); 
            if (input.length > 16) {
                input = input.slice(0, 16); 
            }
            input = input.match(/.{1,4}/g)?.join('-') || input; 
            tarjetaNumero = input;
    }

    function formatearDNI(event: Event) {
        const target = event.target as HTMLInputElement | null;
        if (!target) return; 
        let input = target.value.replace(/\D/g, ''); 
        if (input.length > 8) {
            input = input.slice(0, 8); 
        }
        if (input.length > 5) {
            input = input.slice(0, 2) + '.' + input.slice(2, 5) + '.' + input.slice(5);
        } else if (input.length > 2) {
            input = input.slice(0, 2) + '.' + input.slice(2);
        }
        cliente.dni = input;
    }
</script>

<style>
    .form-container {
        background-color: #888686;
        padding: 40px;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 50px auto;
        max-width: 600px;
        display: block; 
    }

    .form-container h2 {
        margin-bottom: 20px;
    }

    .form-container label {
        margin-top: 10px;
    }

    .form-container input, .form-container select {
        margin-bottom: 10px;
        padding: 8px;
        width: 100%;
        max-width: 300px;  
        border-radius: 5px;
        border: 1px solid #ccc;
    }

    .form-container button {
        margin-top: 20px;
        padding: 10px 20px;
        background-color: #4CAF50;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }

    .form-container button:hover {
        background-color: #45a049;
    }

    .factura-container {
        background-color: #d0cfd1;
        padding: 40px;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 50px auto;
        max-width: 600px;
    }

    .factura-container h2 {
        margin-bottom: 20px;
    }

    .factura-container p {
        margin: 10px 0;
    }

    .factura-container button {
        background-color: #4CAF50;
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 5px;
        cursor: pointer;
    }
    form div {
    display: flex;
    flex-direction: column;
    margin-bottom: 1rem;
  }

  label {
    margin-bottom: 0.5rem;
    font-weight: bold;
  }

  input {
    padding: 0.5rem;
    font-size: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  header {
    width: 100%;
    display: flex;
    margin-right: auto; 
    padding: 1rem 2rem;
    background-color: #f5f5f5;
    border-bottom: 1px solid #ddd;
    box-sizing: border-box;
  }

  .home-button {
    margin-left: auto; 
  }

  .icon {
    width: 35px;
    height: 35px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    background-color: #007bff; 
    color: white;
    font-size: 1.2rem;
    font-weight: bold;
  }

  .user-info {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
</style>

<header>
    <div class="user-info">
      <span>{username}</span>
      <div class="icon">U</div>
    </div>
    <button class="home-button" on:click={() => goto('/Home')}>Inicio</button>
  </header>

{#if !facturaGenerada}
    <div class="form-container">
            <h2>Generar Factura</h2>

            <form on:submit|preventDefault={generarFactura}>
                <label for="patente">Seleccionar Patente:</label>
                <select id="patente" bind:value={patenteSeleccionada} on:change={actualizarDetallesVehiculo} required>
                    <option value="">Seleccione una patente</option>
                    {#each rtos as rto}
                        <option value={rto.patente}>{rto.patente} - {rto.marca} {rto.modelo}</option>
                    {/each}
                </select>

                <!--
                    
                -->
                <!--{#if patenteSeleccionada}-->
                    <div>
                        <h3>Datos de facturacion</h3>

        
                        <div>
                            <label for="clienteNombre">Nombre:</label>
                            <input type="text" id="clienteNombre" bind:value={cliente.nombre} placeholder="Nombre del cliente" required />
                        </div>

                        <div>
                            <label for="clienteDni">DNI:</label>
                            <input type="text" id="clienteDni" bind:value={cliente.dni} placeholder="DNI del cliente" maxlength="11" on:input={formatearDNI} required />
                        </div>

                        <div>
                            <label for="clienteTelefono">Teléfono:</label>
                            <input type="tel" id="clienteTelefono" bind:value={cliente.telefono} placeholder="Teléfono del cliente" maxlength="10" required />
                        </div>

                        <h3>Detalles del Vehículo</h3>
                        
                        <div>
                            <label for="vehiculoMarca">Marca:</label>
                            <input type="text" id="vehiculoMarca" bind:value={vehiculo.marca} readonly />
                        </div>

                        <div>
                            <label for="vehiculoModelo">Modelo:</label>
                            <input type="text" id="vehiculoModelo" bind:value={vehiculo.modelo} readonly />
                        </div>

                        <div>
                            <label for="vehiculoAño">Año:</label>
                            <input type="text" id="vehiculoAño" bind:value={vehiculo.año} readonly />
                        </div>

                        <div>
                            <label for="vehiculoTipo">Tipo:</label>
                            <input type="text" id="vehiculoTipo" bind:value={vehiculo.tipo} readonly />
                        </div>

                        <h3>Método de Pago</h3>
                        <select id="metodoPago" bind:value={metodoPago} required>
                            <option value="">Seleccione un método de pago</option>
                            <option value="Efectivo">Efectivo</option>
                            <option value="Debito">Debito</option>
                        </select>

                        {#if metodoPago === 'Debito'}
                            <p>Datos de tarjeta</p>
                            <div>
                                <label for="tarjetaNumero">Número de tarjeta:</label>
                                <input type="text" id="tarjetaNumero" placeholder="Número de tarjeta" bind:value={tarjetaNumero} on:input={formatearNumeroTarjeta} required />

                                <label for="tarjetaVencimiento">Fecha de vencimiento:</label>
                                <input 
                                type="text" id="tarjetaVencimiento" placeholder="MM/AA" maxlength="5" bind:value={fecha} on:input={formatFechaVencimiento} required
                                >

                                <label for="tarjetaCvv">CVV:</label>
                                <input type="text" id="tarjetaCvv" placeholder="CVV" maxlength="3" required/>

                            </div>
                        {/if}
                        <h3>Monto: {monto}</h3>

                        <button type="submit">Confirmar Pago</button>
                    </div>
                <!--{/if}-->
            </form>    
    </div>
{/if}    

{#if facturaGenerada}
    <div class="factura-container">
        <h2>Factura Generada</h2>
        <p><strong>Patente:</strong> {patenteSeleccionada}</p>
        <p><strong>Marca:</strong> {vehiculo.marca}</p>
        <p><strong>Modelo:</strong> {vehiculo.modelo}</p>
        <p><strong>Año:</strong> {vehiculo.año}</p>
        <p><strong>Cliente:</strong> {cliente.nombre}</p>
        <p><strong>DNI:</strong> {cliente.dni}</p>
        <p><strong>Teléfono:</strong> {cliente.telefono}</p>
        <p><strong>Método de Pago:</strong> {metodoPago}</p>
        <p><strong>Monto:</strong> {monto}</p>
        <button on:click={descargarComprobante}>Descargar Comprobante</button>
        <br>
        <button on:click={() => facturaGenerada = false}>Generar otra factura</button>
    </div>
{/if}