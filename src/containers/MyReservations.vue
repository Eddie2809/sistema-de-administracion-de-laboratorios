<script>
    /* 
        Asignado a: Bryan

        Notas:
        -Reservaciones pendientes son aquellas con estado 2
        -Reservaciones activas con aquellas con estado 1
        -Reservaciones inactivas son aquellas con estado 0

        Funciones útiles:
        -No hai sorri unu
    */
</script>

<template>
    <div class="container MyReservations">
        <div class="body-style">
            <div class="Titulo">
                <h1>Mis reservaciones</h1>
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
                            <p>Laboratorio solicitado:</p>
                            <p>Laboratorio de ngenieria  de software{{Nombre_Profesor}}</p>
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
                            <p>Laboratorio solicitado:</p>
                            <p>Laboratorio de ngenieria  de software{{Nombre_Profesor}}</p>
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