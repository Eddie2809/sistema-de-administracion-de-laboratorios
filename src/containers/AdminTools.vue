<script>
import { Component } from "preact"

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
    import ToolsDocente from './ToolsDocente.vue'
    import ToolsLabs from './ToolsLabs.vue'
    import ToolsReservation from './ToolsReservation.vue'

export default {
    data(){
        return{
            adminOption: "docentes"
        }
    },
    methods: {
        changeOption(newOption) {
            this.adminOption = newOption
        }
    },
    components: {
        ToolsDocente,
        ToolsLabs,
        ToolsReservation
    },
    props: ["lablist", "userlist", "getUsers", "getLabs", "assignLabManager", "deleteLab", "modifyLab", "addNewLab", "addNewUser", "modifyUser", "deleteUser", "changeLabStatus", "getEvents", "getReservations", "reservations", "userData"]
    
}
</script>

<template>
    <div class="container AdminTools">
        <h1 class="title">Herramientas de administrador</h1>
        <div class="menu-content">
            <div class="side-menu"> 
                <a @click="changeOption('laboratorios')" href="#">Laboratorios</a>
                <a @click="changeOption('docentes')" href="#">Docentes</a>
                <a @click="changeOption('reservacion')" href="#">Reservaciones</a>
            </div>
            
            <ToolsDocente v-if="this.adminOption === 'docentes'" :getUsers="getUsers" :userlist="this.userlist" :addNewUser="addNewUser" :modifyUser="modifyUser" :deleteUser="deleteUser"/>
            <ToolsLabs v-if="this.adminOption === 'laboratorios'" :lablist="this.lablist" :getLabs="getLabs" :assignLabManager="assignLabManager" :deleteLab="deleteLab" :modifyLab="modifyLab" :addNewLab="addNewLab"/>
            <ToolsReservation v-if="this.adminOption === 'reservacion'" :changeLabStatus="changeLabStatus" :getEvents="getEvents" :getReservations="getReservations" :reservations="this.reservations" :userData="this.userData"/>

        </div>
        
    </div>
</template>