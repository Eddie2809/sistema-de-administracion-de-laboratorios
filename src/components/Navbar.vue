<script>
    export default{
        props: ['changeRoute','userType','route','logOut'],
    }
</script>

<template>
    <nav class="navbar">
        <img src="../assets/logo-sin-subtitulo-claro.png" alt="">
        <ul>
            <li :class="this.route === 'home'? 'selected':null" v-if="this.userType === 'null' || this.userType === 'administrador' || this.userType === 'docente' || this.userType === 'encargado'" @click="this.changeRoute('home')">Consultar horarios</li>
            <li :class="this.route === 'labslist'? 'selected':null" v-if="this.userType === 'administrador' || this.userType === 'null' || this.userType === 'docente' || this.userType === 'encargado'" @click="this.changeRoute('labslist')">Lista de laboratorios</li>
            <li :class="this.route === 'reservationrequest'? 'selected':null" v-if="this.userType === 'administrador' || this.userType === 'docente' || this.userType === 'encargado'" @click="this.changeRoute('reservationrequest')">Hacer solicitud de reservación</li>
            <li :class="this.route === 'myreservations'? 'selected':null" v-if="this.userType === 'administrador' || this.userType === 'docente' || this.userType === 'encargado'" @click="this.changeRoute('myreservations')">Ver mis reservaciones</li>
            <li :class="this.route === 'managereservations'? 'selected':null" v-if="this.userType === 'encargado'" @click="this.changeRoute('managereservations')">Administrar solicitudes</li>
            <li :class="this.route === 'admintools'? 'selected':null" v-if="this.userType === 'administrador'" @click="this.changeRoute('admintools')">Herramientas de administrador</li>
            <li :class="this.route === 'login'? 'selected':null" v-if="this.userType !== 'null'" @click="this.logOut()">Cerrar sesión</li>
            <button class="loginButton" v-if="this.userType === 'null'" @click="this.changeRoute('login')">Iniciar sesión</button>
        </ul>
    </nav>
</template>