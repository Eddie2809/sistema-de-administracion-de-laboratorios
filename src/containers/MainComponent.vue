<script>
    import Home from './Home.vue'
    import Login from './Login.vue'
    import LabsList from './LabsList.vue'
    import ManageReservations from './ManageReservations.vue'
    import Navbar from '../components/Navbar.vue'
    import Footer from '../components/Footer.vue'
    import MyReservations from './MyReservations.vue'
    import ReservationRequest from './ReservationRequest.vue'
    import AdminTools from './AdminTools.vue'

    const apiURL = 'https://salunicaribe-api.herokuapp.com/'

    export default ({
        data(){
            return{
                route: 'home',
                userData: {tipo: 'null'},
                labsList: [],
                usersList: [],
                events: [],
                reservations: [],
                reportData: [],
                newUserData: {}
            }
        },
        components: {
            MyReservations,
            Login,
            ReservationRequest,
            Home,
            LabsList,
            ManageReservations,
            Navbar,
            Footer,
            AdminTools
        },
        methods: {

            // IGNORAR
            // ===============================================================================
            async fetchData(route,bodyObject){
                const resp = await fetch(apiURL + route, {
                    method: 'post',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(bodyObject)
                }).then(res => res.json())
                return resp
            },
            async fetchGet(route){
                const resp = await fetch(apiURL + route, {
                    method: 'get',
                    headers: {'Content-Type': 'application/json'}
                }).then(res => res.json())
                return resp
            },

            changeRoute(newRoute){
                this.route = newRoute
            },
            //===============================================================================

                
            // Añade nueva reservación a la base de datos con sus respectivas horas, si no se especifico razón
            // puedes añadir una string vacia, el formato de horas es una lista de objetos, el formato del 
            // objeto es el siguiente:
            /*
                inicio: new Date('yyyy-mm-ddThh:mm:ss')
                final: new Date('yyyy-mm-ddThh:mm:ss')
             */
            createNewReservation(reason,labId,hours){
                this.fetchData('new-reservation',{
                    reason: reason,
                    userId: this.userData.id,
                    labId: labId,
                    hours: hours
                })
                .then(res => {
                    if(res !== 'Éxito') alert('Algo salió mal')
                    else alert('Hecho!')
                })
                .catch(err => {
                    alert('Algo salió mal')
                })
            },

            // Obtiene la lista de reservaciones del laboratorio o usuario especificados, en caso de no querer
            // especificar el usuario o laboratorio pasar -1 de parámetro.
            getReservations(labId,userId){
                this.fetchData('get-reservations',{
                    labId: labId,
                    userId: userId
                })
                .then(reservations => {
                    console.log(reservations)
                    this.reservations = reservations
                })
                .catch(err => alert('Algo salio mal'))
            },

            // Añadir nuevo laboratorio a la base de datos, solo especifica el nombre y la id de encargado, en
            // caso de que no haya todavía un encargado asignado puedes escribir -1 en el campo y se deja vacio
            addNewLab(name,labManager){
                this.fetchData('add-new-lab',{
                    name: name,
                    user: labManager
                })
                .then(() => alert('Hecho'))
            },

            // Añade nuevo usuario a la base de datos, todos los campos tienen que ser llenados, usertypes:
            // 1: Administrador
            // 2: Encargado de laboratorio 
            // 3: Docente
            addNewUser(name,lastname,email,usertype){
                this.fetchData('user-signup',{
                    name: name,
                    lastname: lastname,
                    email: email,
                    usertype: usertype
                })
                .then(res => {
                    this.newUserData = res
                    let msg
                    if(res.contrasena) msg = 'Usuario registrado con la siguiente información:\n\nNombre de usuario: ' + res.nombre_de_usuario + '\nContraseña: ' + res.contrasena + '\n\nIMPORTANTE: ESTE MENSAJE SOLO LO PODRÁ VER UNA VEZ'
                    else msg = 'Hubo un error'
                    alert (msg)
                })
            },

            // Asigna los eventos de un respectivo laboratorio a this.events
            getEvents(labId){
                this.fetchData('get-events',{labId}).then(res => {
                    console.log(res)
                    let events = []
                    res.forEach(ob => {
                        events.push({
                            start: ob.start.slice(0,19),
                            end: ob.end.slice(0,19),
                            title: ob.userName + ' ' + ob.userLastname,
                        })
                    })
                    this.events = events
                })
            },

            // Rechaza o acepta una reservación, si no hay mensaje puedes enviar una string vacia, si se acepta
            // la solicitud, entonces escribir 1 en response, caso contrario escribir 0
            evaluateReservation(reservationId,response,msg){
                this.fetchData('evaluate-reservation',{
                    newStatus: response,
                    reservationId: reservationId,
                    msg: msg
                })
                .then(() => alert('Hecho'))
            },

            // Evalua si el username y la password son correctas, si lo son cambia de ruta y recupera la infor-
            // mación
            evaluateCredentials(username,password){
                this.fetchData('login',{username: username,password: password}).then(res => {
                    if(res === 'Credenciales inválidas'){
                        alert(res)
                    }
                    else{
                        this.route = 'home'
                        this.userData = res
                        alert('Has ingresado')
                    }
                })
            },

            // Te da la capacidad de modificar el nombre o disponibilidad de un laboratorio, en caso de querer
            // modificar solamente uno, puedes escribir en el otro campo el mismo valor, pero no dejar vacio.
            modifyLab(labId,newName,disp){
                this.fetchData('modify-lab',{
                    id: labId,
                    newName: newName,
                    disp: disp
                })
                .then(res => alert('Hecho'))
            },

            // Te da la capacidad de cambiar el encargado de algún laboratorio. Si se desea eliminar un encar-
            // gado, escribir -1 en el campo de newManagerId
            assignLabManager(labId,newManagerId){
                this.fetchData('assign-lab-manager',{
                    labId: labId,
                    userId: newManagerId
                })
                .then(res => {
                    alert(res)
                })
            },

            // Eliminar de la base de datos el laboratorio con su respectivo id
            deleteLab(labId){
                this.fetchData('delete-lab',{labId: labId}).then(res => alert('Hecho'))
            },

            // Asigna la lista de laboratorios con sus respectivos encargados a this.labsList
            getLabs(){
                this.fetchGet('get-labs').then(res => {
                    console.log(res)
                    this.labsList = res
                })
            },

            // Asigna a this.usersList la lista de los usuarios registrados en la base de datos.
            getUsers(){
                this.fetchGet('get-users').then(res => {
                    this.usersList = res
                })
            },

            // Elimina un usuario de la base de datos utilizando su id
            deleteUser(userId){
                this.fetchData('delete-user',{userId: userId})
                .then(res => alert(res))
                .catch(err => alert(err))
            },

            // Modifica los datos de un usuario, en los campos de los datos que no se vayan a cambiar tiene que
            // especificarse el dato actual. Se recomienda solo usar newUserType en caso de querer hacer admi-
            // nistrador a un usuario
            modifyUser(userId,newName,newLastname,newEmail, newPassword,newUsername,newUsertype){
                this.fetchData('modify-user',{
                    userId: userId,
                    newName: newName,
                    newLastname: newLastname,
                    newEmail: newEmail,
                    newPassword: newPassword,
                    newUsername: newUsername,
                    newUsertype: newUsertype
                })
                .then(res => alert('Hecho'))
            },

            // Obtiene todos los datos relacionados a las horas existentes de un laboratorio especificado, en
            // caso de querer obtener los datos de todos los laboratorios escribir -1
            getReportData(labId){
                this.fetchData('get-report-data',{labId: labId}).then(res => {
                    this.reportData = res
                    console.log(res)
                })
            },

            // Cambia la disponibilidad de un laboratorio (True si disponible, false si no disponible)
            changeLabStatus(labId,newStatus){
                this.fetchData('switch-lab-status',{status: newStatus,labId: labId})
                .then(() => alert('Hecho'))
            },

            // Cancela la reservación con la id reservationId
            cancelReservation(reservationId){
                this.fetchData('cancel-reservation',{reservationId: reservationId})
                .then(() => alert('Hecho'))
            },

            // Cierra sesión
            logOut(){
                this.userData = {tipo: 'null'}
                this.route = 'home'
            }
        }
    })
</script>

<template>
    <div class="MainComponent">
        <Navbar v-if="this.route !== 'login'" :changeRoute="changeRoute" :userType="userData.tipo" :route="this.route" :logOut="logOut"/> 
        <Login v-if="this.route === 'login'" :evaluateCredentials="evaluateCredentials"/>
        <AdminTools v-if="this.route === 'admintools'"/>
        <Home v-if="this.route === 'home'" :getLabs="getLabs" :getEvents="getEvents" :labsList="this.labsList" :events="events"/>
        <LabsList v-if="this.route === 'labslist'" :getLabs="getLabs" :list="this.labsList" />
        <ManageReservations v-if="this.route === 'managereservations'"  :getLabs="getLabs" :userData="this.userData" :reservations="this.reservations" :labsList="this.labsList" :getReservations="getReservations"/>
        <MyReservations v-if="this.route === 'myreservations'"  :userData="this.userData" :reservations="this.reservations" :getReservations="getReservations"/>
        <ReservationRequest v-if="this.route === 'reservationrequest'"/>
        <Footer v-if="this.route !== 'login'"/>
    </div>
</template>

<style>
</style>
