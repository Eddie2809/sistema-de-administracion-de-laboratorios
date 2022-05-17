<script>
/* 
    Asignado a: Eduardo y Erick

    A tomar en cuenta:

    -Reservaciones es casi la misma pantalla que 'Administrar reservaciones', ponerse de acuerdo para reciclar
    componentes.
    -Al eliminar usuario o laboratorio siempre abrir una ventana de confirmación
    -Cargar componentes de la barra de navegación izquierda dentro de este mismo componente

    Funciones útiles:
    -changeLabStatus (Para administrar solicitudes)
    -getEvents (Para generar vista previa)
    -getReservations
*/
import Table from "../components/Table.vue"


export default{
    data(){
        return{
            input: ''
        }
    },
    components: {
        Table
    },
    props: ["changeLabStatus", "getEvents", "getReservations", "reservations", "userData"],
    mounted() {
        this.getReservations(-1, this.userData.id)
    }
}
</script>

<template>
    <div> 
        <h2 class="subtitle">Reservaciones</h2>
        <div class="add-search">
            <button id="agg" href="#">Administrar solicitudes</button>
        </div>
        
        <div>
            <div class="search-bar">
                <h2>Solicitudes pendientes</h2>
                <p>Buscar:</p>
                <input type="text" v-model="input" placeholder="Nombre de docente o laboratorio">
            </div>
            <Table :reservations = reservations :reservationStatus = 2 :tableHeaderType = 2 :tableBodyType = 1 :tableButtonType = 0 />

        </div>



    </div>
    
</template>