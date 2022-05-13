<script>
    /* 
        Asignado a: Bryan

        Notas: 
        -Abrir ventana de confirmación para cancelar reservación
        -Abrir ventana para escribir motivo de rechazo al rechazar una reservación
        -La vista previa es para que el encargado vea como quedaría el horario si aceptara esa reservación
        -En 'Estado' puedes permitir o rechazar las solicitudes de reservación
        -Reservaciones activas son aquellas con estado 1
        -Reservaciones inactivas son aquellas con estado 0
        -Esta pendiente la función para obtener las reservaciones :(

        Funciones útiles:
        -evaluateReservation
        -getReportData
        -cancelReservation
        -getReservations
    */
</script>

<template>
    <div class="container ManageReservations">
        <div class="body-style">
            <div class="Titulo">
                <h1>Solicitudes de Reservación</h1>
                <button>Generar reporte</button>
            </div>

            <div class="Lab_Options">
                <h3>Laboratorio asignado: {{Lab}}</h3>
                <div class="Lab_Status">
                <p>Estado:</p>
                    <select>
                        <option>Se permiten solicitudes</option>
                        <option>No se permiten solicitudes</option>
                    </select>
                    <button>Actualizar</button>
                </div>
            </div>

            <div class="search-style">
                <h2>Solicitudes Pendientes</h2>
                <p>Buscar:</p>
                <input type="text" placeholder="Nombre de docente">
            </div>

            <table class="table_Reservaciones">
                <thead>
                    <tr>
                        <th class="table_header1">
                            <p>Genero la solicitud: Hector Fernando Gómez García {{Nombre_Profesor}}</p>
                            <button>Generar vista previa</button>
                        </th>
                        <th class="table_header2">
                            <strong>Razon:</strong>
                            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus egestas, risus eget interdum viverra, metus nisi consequat urna, vitae maximus ipsum turpis quis ante. {{Motivo_Reservacion}}</p>
                        </th>
                        <th class="table_header3">
                            <ul>
                                <li>
                                    <p>Fecha de solicitud: 10/10/2020{{Fecha_Reservacion}}</p>
                                    <p>Cantidad por semanas: 3{{Semanas_Reservacion}}</p>
                                    <p>Reservar por todo el semestre: Si{{Semestre_Reservar}}</p>
                                </li>
                            </ul>
                        </th>
                    </tr>
                </thead>

                <tbody>
                    <td>Dia: {{Dia_Rerservacion}}</td>
                    <td>Hora de inicio: {{Hora_Inicio}}</td>
                    <td>Hora de fin: {{Hora_Fin}}</td>
                </tbody>
                <tbody>
                    <td>Dia: {{Dia_Rerservacion}}</td>
                    <td>Hora de inicio: {{Hora_Inicio}}</td>
                    <td>Hora de fin: {{Hora_Fin}}</td>
                </tbody>
                <tbody>
                    <td>Dia: {{Dia_Rerservacion}}</td>
                    <td>Hora de inicio: {{Hora_Inicio}}</td>
                    <td>Hora de fin: {{Hora_Fin}}</td>
                </tbody>
                <div class="buttons">
                    <button class="green_button">Aprobar todo</button>
                    <button class="red_button">Rechazar todo</button>
                </div>
            </table>

            <div class="search-style">
                <h2>Reservaciones activas</h2>
                <p>Buscar:</p>
                <input type="text" placeholder="Nombre de docente">
            </div>
            
            <table class="table_Reservaciones">
                <thead>
                    <tr>
                        <th class="table_header1">
                            <p>Genero la solicitud: {{Nombre_Profesor}}</p>
                            <button>Generar vista previa</button>
                        </th>
                        <th class="table_header2">
                            <p>Razón:</p>
                            <p>{{Motivo_Reservacion}}</p>
                        </th>
                        <th class="table_header3">
                            <ul>
                                <li>
                                    <p>Fecha de solicitud: {{Fecha_Reservacion}}</p>
                                    <p>Cantidad por semanas: {{Semanas_Reservacion}}</p>
                                    <p>Reservar por todo el semestre: {{Semestre_Reservar}}</p>
                                </li>
                            </ul>
                        </th>
                    </tr>
                </thead>

                <tbody>
                    <td>Dia: {{Dia_Rerservacion}}</td>
                    <td>Hora de inicio: {{Hora_Inicio}}</td>
                    <td>Hora de fin: {{Hora_Fin}}</td>
                </tbody>
                <tbody>
                    <td>Dia: {{Dia_Rerservacion}}</td>
                    <td>Hora de inicio: {{Hora_Inicio}}</td>
                    <td>Hora de fin: {{Hora_Fin}}</td>
                </tbody>
                <tbody>
                    <td>Dia: {{Dia_Rerservacion}}</td>
                    <td>Hora de inicio: {{Hora_Inicio}}</td>
                    <td>Hora de fin: {{Hora_Fin}}</td>
                </tbody>
                <div class="buttons">
                    <button class="cancel_button">Cancelar reservacion</button>
                </div>
            </table>
        </div>
    </div>
</template>