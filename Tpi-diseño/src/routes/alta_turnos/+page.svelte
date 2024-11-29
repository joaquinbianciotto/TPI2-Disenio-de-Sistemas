<script>
  import { writable } from "svelte/store";
  import { addDays, format, startOfWeek, endOfWeek, getMonth } from "date-fns";

  const currentDate = writable(new Date()); // Fecha actual
  /**
   * @type {number | null}
   */
  let selectedSlot = null; // Turno seleccionado
  let occupiedSlots = new Map(); // Turnos ocupados
  let showForm = false; // Controla la visibilidad del formulario
  let clientData = { name: "", dni: "", plate: "", model: "", brand: "" }; // Datos del cliente

  // Calcular inicio y fin de semana
  $: startDate = startOfWeek($currentDate, { weekStartsOn: 1 });
  $: endDate = endOfWeek($currentDate, { weekStartsOn: 1 });

  // Generar horas
  const hours = Array.from({ length: 12 }, (_, i) => `${7 + i}:00`);

  // Generar turnos ocupados dinámicamente al cambiar el mes
  $: {
    const month = getMonth($currentDate); // Mes actual
    occupiedSlots = generateOccupiedSlots(month); // Regenerar turnos ocupados al cambiar de mes
  }

  // Generar turnos ocupados aleatoriamente para un mes específico
  /**
   * @param {number} month
   */
  function generateOccupiedSlots(month) {
    const totalSlots = 42; // Total de slots en una semana
    const seed = month; // Usamos el mes como semilla para garantizar unicidad por mes
    const random = seededRandom(seed);
    const numOccupied = Math.floor(random() * 10) + 10; // Entre 10 y 20 ocupados
    const occupied = new Map();

    while (occupied.size < numOccupied) {
      const slotIndex = Math.floor(random() * totalSlots);
      if (!occupied.has(slotIndex)) {
        occupied.set(slotIndex, {
          ...generateClientData(),
          date: getSlotDetails(slotIndex).date, // Agregar la fecha al turno
          hour: getSlotDetails(slotIndex).hour, // Agregar la hora al turno
        });
      }
    }
    return occupied;
  }

  // Generar datos ficticios del cliente
  function generateClientData() {
    const names = [
      "Juan Pérez",
      "María García",
      "Carlos López",
      "Ana Torres",
      "Pedro Gómez",
      "Laura Díaz",
      "Luis Fernández",
      "Sofía Martínez",
      "Agustín Martínez",
      "Valentina Pérez",
      "Emiliano Gómez",
      "Camila López",
      "Santiago Torres",
      "Isabela Díaz",
      "Mateo Fernández",
      "Lucía Sánchez",
      "Thiago González",
      "Martina Ramírez",
      "Sebastián Morales",
      "Antonella Herrera",
      "Benjamín Vargas",
      "Sofía Castillo",
      "Nicolás Rojas",
      "Julieta Jiménez",
      "Joaquín Romero",
      "Abril Aguirre",
      "Valentín Méndez",
      "Renata Ortiz",
      "Tomás Medina",
      "Malena Paredes",
      "Gabriel Salinas",
      "Emma Suárez",
      "Bruno Castro",
      "Catalina Ortega",
      "Lucas Silva",
      "Carolina Espinoza",
      "Facundo Núñez",
      "Zoe Sandoval",
      "Damián Carrillo",
      "Elena Navarro",
      "Diego Peña",
      "Victoria Guzmán",
      "Matías Delgado",
      "Alma Vega",
      "Felipe Cabrera",
      "Paula Benítez",
      "Ian Álvarez",
      "Andrea Montes",
      "Francisco Molina",
      "Mía Araya",
      "Kevin Ruiz",
      "Florencia Chávez",
      "Iván Correa",
      "Mariana Miranda",
      "Cristian Campos",
      "Pilar Calderón",
      "Carlos Fuentes",
      "Lara Valenzuela",
      "Ángel Parra",
      "Eva Tapia",
      "Pablo Villarreal",
      "Carla Reyes",
      "Ramiro Villalobos",
      "Milena Gutiérrez",
      "Andrés Robles",
      "Bárbara León",
      "Franco Sosa",
      "Luana Valverde",
      "Agustina Guerrero",
      "Sergio Cárdenas",
      "Martín Chávez",
      "Daniela Figueroa",
      "Gonzalo Duarte",
      "Laura Beltrán",
      "Hugo Orozco",
      "Lorena Ávila",
      "Ezequiel Acevedo",
      "Jimena Ponce",
      "Manuel Flores",
      "Adriana Peralta",
      "Julián Escobar",
      "Clara Serrano",
      "Simón Paz",
      "Carina Muñoz",
      "Ramón Maldonado",
      "Gabriela Alfaro",
      "Marcos Solano",
      "Estefanía Gallardo",
      "Federico Santana",
      "Bianca Arroyo",
      "Javier Ortiz",
      "Alicia Cáceres",
      "Ricardo Rivas",
      "Samantha Camacho",
      "Oscar Quintana",
      "María Vargas",
      "Pedro Prieto",
      "Luciana Márquez",
      "Rodrigo Zambrano",
      "Camila Alarcón",
      "Vicente Esquivel",
      "Natalia Trujillo",
      "Raúl Valdés",
      "Silvana Villena",
    ];
    const randomName = names[Math.floor(Math.random() * names.length)];
    const randomPlate = `${randomLetter()}${randomLetter()}${randomLetter()}${Math.floor(Math.random() * 900) + 100}`;
    return { name: randomName, plate: randomPlate };
  }

  // Generar letra aleatoria para patentes
  function randomLetter() {
    const letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    return letters[Math.floor(Math.random() * letters.length)];
  }

  // Generador de números aleatorios con semilla
  /**
   * @param {number} seed
   */
  function seededRandom(seed) {
    let value = seed;
    return () => {
      value = (value * 9301 + 49297) % 233280;
      return value / 233280;
    };
  }

  // Cambiar semana
  /**
   * @param {number} offset
   */
  function changeWeek(offset) {
    currentDate.update((date) => addDays(date, offset * 7));
  }

  // Seleccionar un turno disponible
  /**
   * @param {number | null} index
   */
  function selectSlot(index) {
    if (occupiedSlots.has(index)) return; // Si está ocupado, no permite seleccionar
    selectedSlot = index;
    showForm = true; // Mostrar formulario
  }

  // Confirmar turno
  function confirmSlot() {
    const slotDetails = getSlotDetails(selectedSlot);

    // Marcar el turno como ocupado
    occupiedSlots.set(selectedSlot, {
      name: clientData.name,
      plate: clientData.plate,
      date: slotDetails.date, // Agregar la fecha
      hour: slotDetails.hour, // Agregar la hora
    });

    alert(
      `Turno confirmado:\nDía: ${slotDetails.day}\nFecha: ${slotDetails.date}\nHora: ${slotDetails.hour}\n\n` +
        `Datos del Cliente:\nNombre: ${clientData.name}\nDNI: ${clientData.dni}\nPatente: ${clientData.plate}\nModelo: ${clientData.model}\nMarca: ${clientData.brand}`
    );

    // Reiniciar datos y cerrar formulario
    clientData = { name: "", dni: "", plate: "", model: "", brand: "" };
    showForm = false;
    selectedSlot = null;
  }

  // Cerrar modal
  function closeModal() {
    showForm = false;
    selectedSlot = null;
  }

  // Obtener detalles del turno seleccionado
  /**
   * @param {number | null} slotIndex
   */
  function getSlotDetails(slotIndex) {
    // Verificar si slotIndex es nulo o un valor inválido
    if (
      slotIndex === null ||
      slotIndex < 0 ||
      slotIndex >= hours.length * 7 // 7 días en la semana
    ) {
      throw new Error("El índice del turno (slotIndex) es inválido.");
    }

    const dayIndex = Math.floor(slotIndex / hours.length); // Indice del día
    const hourIndex = slotIndex % hours.length; // Indice de la hora

    const dayDate = addDays(startDate, dayIndex); // Calcular la fecha del día

    return {
      date: format(dayDate, "dd/MM/yyyy"),
      hour: hours[hourIndex],
      day: `${format(dayDate, "EEEE")} ${format(dayDate, "dd")}`, // Día con número
    };
  }
</script>

<div class="container mt-4">
  <div class="d-flex justify-content-between align-items-center mb-3">
    <button class="btn btn-primary" on:click={() => changeWeek(-1)}
      >Anterior</button
    >
    <h1 class="text-center">
      Semana del {format(startDate, "dd/MM/yyyy")} al {format(
        endDate,
        "dd/MM/yyyy"
      )}
    </h1>
    <button class="btn btn-primary" on:click={() => changeWeek(1)}
      >Siguiente</button
    >
  </div>

  <div class="table-responsive">
    <table class="table calendar">
      <thead>
        <tr>
          <th scope="col">Hora</th>
          {#each Array(6) as _, dayIndex}
            <th scope="col">{getSlotDetails(dayIndex * hours.length).day}</th>
          {/each}
        </tr>
      </thead>
      <tbody>
        {#each hours as hour, hourIndex}
          <tr>
            <th>{hour}</th>
            {#each Array(6) as _, dayIndex}
              <td
                class="{occupiedSlots.has(hourIndex + dayIndex * hours.length)
                  ? 'occupied'
                  : ''} {selectedSlot === hourIndex + dayIndex * hours.length
                  ? 'selected'
                  : ''}"
                on:click={() => selectSlot(hourIndex + dayIndex * hours.length)}
              >
                {#if occupiedSlots.has(hourIndex + dayIndex * hours.length)}
                  Nombre: {occupiedSlots.get(
                    hourIndex + dayIndex * hours.length
                  ).name}<br />
                  Patente: {occupiedSlots.get(
                    hourIndex + dayIndex * hours.length
                  ).plate}<br />
                  Fecha: {occupiedSlots.get(hourIndex + dayIndex * hours.length)
                    .date}<br />
                {/if}
              </td>
            {/each}
          </tr>
        {/each}
      </tbody>
    </table>
  </div>

  {#if showForm}
    <div class="overlay" on:click={closeModal}></div>
    <div class="modal">
      <h4>Ingresar Datos del Cliente</h4>
      <label>Nombre y Apellido</label>
      <input type="text" class="form-control" bind:value={clientData.name} />

      <label>DNI</label>
      <input type="text" class="form-control" bind:value={clientData.dni} />

      <label>Patente</label>
      <input type="text" class="form-control" bind:value={clientData.plate} />

      <label>Modelo</label>
      <input type="text" class="form-control" bind:value={clientData.model} />

      <label>Marca</label>
      <input type="text" class="form-control" bind:value={clientData.brand} />

      <button class="btn btn-success" on:click={confirmSlot}
        >Confirmar Turno</button
      >
      <button class="btn btn-secondary" on:click={closeModal}>Cancelar</button>
    </div>
  {/if}
</div>

<style>
  .calendar {
    background-color: #e3f2fd; /* Celeste claro */
    border-radius: 8px;
    padding: 10px;
  }

  .calendar th,
  .calendar td {
    text-align: center;
    border: 1px solid #90caf9;
    padding: 10px;
  }

  .calendar td:hover {
    background-color: #bbdefb; /* Celeste más claro */
  }

  .selected {
    background-color: #64b5f6 !important;
    color: white;
  }

  .occupied {
    background-color: #ddd !important;
    cursor: not-allowed;
    font-size: 12px;
    padding: 5px;
    line-height: 1.2;
  }

  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    padding: 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    z-index: 1000;
    width: 90%;
    max-width: 500px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 999;
  }
</style>
