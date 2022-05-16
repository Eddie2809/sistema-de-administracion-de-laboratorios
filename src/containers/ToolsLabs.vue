<script>
/* 
    Asignado a: Eduardo y Erick

    A tomar en cuenta:

    -Reservaciones es casi la misma pantalla que 'Administrar reservaciones', ponerse de acuerdo para reciclar
    componentes.
    -Al eliminar usuario o laboratorio siempre abrir una ventana de confirmación
    -Cargar componentes de la barra de navegación izquierda dentro de este mismo componente

    Funciones útiles:
    -getLabs
    -assignLabManager
    -deleteLab
    -modifyLab
    -addNewLab
    -addNewUser
    -modifyUser
    -getUsers
    -deleteUser
    -changeLabStatus (Para administrar solicitudes)
    -getEvents (Para generar vista previa)
    -getReservations
*/
    
</script>

<template>
    <div> 
        <h2 class="subtitle">Laboratorios</h2>

        <form action="" id="aggdoc">
            <input type="text" name="name" class="name" placeholder="Nombre" v-model="name">
            <input type="text" name="lastname" class="lastname" placeholder="Apellido" v-model="lastname">
            <input type="text" name="email" class="email" placeholder="E-mail" v-model="email">
            <input type="text" name="usertype" class="usertype" placeholder="Tipo de usuario" v-model="usertype">
        </form>
        <div class="add-search">
            <button @click="this.addNewUser(this.name,this.lastname,this.email.this.usertype)" id="agg" href="#">Agregar laboratorio</button>
            <p>Buscar:</p>
            <input type="text" id="write" placeholder="Por nombre, encargado o correo" v-model="username">

        </div>
        
        <table>
            <tr>
                <th>Nombre</th>
                <th>Encargado</th>
                <th>Correo</th>
                <th>Opciones</th>
            </tr>
            <tr>
                <td>Laboratorio de ingenieria</td>
                <td>Yarely Baez Lopez</td>
                <td>ybaez@ucaribe.edu.mx</td>
                <td>
                    <button @click="this.eliminates" class="asignar">Asignar encargado</button>
                    <button @click="this.modify" class="modificar">Modificar</button> 
                    <button @click="this.eliminates" class="eliminar">Eliminar</button>
                </td>
            </tr>
            
        </table>
    </div>
    
</template>