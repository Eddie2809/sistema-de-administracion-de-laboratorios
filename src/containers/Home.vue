<script>
    import Calendar from '../components/Calendar.vue'

    export default ({
        props: ['getLabs','events','getEvents','labsList'],
        mounted(){
            this.getLabs()
        },
        components: {
            Calendar
        },
        data(){
            return({
                labId: -1
            })
        },
        watch: {
            labId(){
                this.getEvents(this.labId)
            }
        }
    })
/* 
    Asignado a: Eddie    

    Funciones útiles:
    -getEvents
*/
</script>

<template>
    <div class="container Home">
        <h1 class="title">Bienvenido(a) al Sistema de Administración de Laboratorios</h1>
        <p class="subtitle">Ver horarios de laboratorio</p>
        <div class="labInput">
            <p>Laboratorio: </p>
            <select v-model="labId">
                <option value="-1">--Selecciona un laboratorio--</option>
                <option v-for="labs in labsList" :key="labs.id_laboratorio" :value="labs.id_laboratorio">{{labs.nombre_laboratorio}}</option>
            </select>
        </div>
        <hr>
        <Calendar :events="this.events"/>
    </div>
</template>