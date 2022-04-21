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

    export default{
        data(){
            return{
                route: 'home',
                userData: {tipo: 'null'},
                labsList: [],
                usersList: []
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
            createNewReservation(reason,labId,hours){
                this.fetchData('new-reservation',{
                    reason: reason,
                    userId: this.userId,
                    labId: labId,
                    hours: hours
                })
                .then(() => alert('Hecho'))
                .catch(err => {
                    alert('Algo salió mal')
                })
            },
            addNewLab(name,labManager){
                this.fetchData('add-new-lab',{
                    name: name,
                    user: labManager
                })
                .then(() => alert('Hecho'))
            },
            addNewUser(name,lastname,email,usertype){
                this.fetchData('user-signup',{
                    name: name,
                    lastname: lastname,
                    email: email,
                    usertype: usertype
                })
                .then(() => alert('Hecho'))
            },
            getEvents(labId){
                this.fetchData('get-events',{labId}).then(res => console.log(res))
            },
            evaluateReservation(reservationId,response,msg){
                this.fetchData('evaluate-reservation',{
                    newStatus: response,
                    reservationId: reservationId,
                    msg: msg
                })
                .then(() => alert('Hecho'))
            },
            evaluateCredentials(username,password){
                this.fetchData('login',{username: username,password: password}).then(res => {
                    if(res === 'Credenciales inválidas'){
                        alert(res)
                    }
                    else{
                        this.route = 'home'
                        this.userData = res
                        console.log(res)
                        alert('Has ingresado')
                    }
                })
            },
            modifyLab(labId,newName,disp){
                fetchData('modify-lab',{
                    id: labId,
                    newName: newName,
                    disp: disp
                })
                .then(res => alert('Hecho'))
            },
            assignLabManager(labId,newManagerId){
                this.fetchData('assign-lab-manager',{
                    labId: labId,
                    userId: newManagerId
                })
                .then(res => alert('Hecho'))
            },
            deleteLab(labId){
                this.fetchData('delete-lab',{labId: labId}).then(res => alert('Hecho'))
            },
            getLabs(){
                this.fetchGet('get-labs').then(res => {
                    this.labsList = res
                })
            },
            getUsers(){
                this.fetchGet('get-users').then(res => {
                    this.usersList = res
                })
            },
            deleteUser(userId){
                this.fetchData('deleteUser',{userId: userId})
                .then(res => alert('Hecho'))
            },
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
            getReportData(data){
                this.fetchGet('get-report-data').then(res => data = res)
            },
            changeLabStatus(labId,newStatus){
                this.fetchData('switch-lab-status',{status: newStatus,labId: labId})
                .then(() => alert('Hecho'))
            },
            cancelReservation(reservationId){
                this.fetchData('cancel-reservation',{reservationId: reservationId})
                .then(() => alert('Hecho'))
            },
            logOut(){
                this.userData = {tipo: 'null'}
                this.route = 'home'
            }
        }
    }
</script>

<template>
    <div class="MainComponent">
        <Navbar v-if="this.route !== 'login'" :changeRoute="changeRoute" :userType="userData.tipo" :route="this.route" :logOut="logOut"/> 
        <Login v-if="this.route === 'login'" :evaluateCredentials="evaluateCredentials"/>
        <AdminTools v-if="this.route === 'admintools'"/>
        <Home v-if="this.route === 'home'"/>
        <LabsList v-if="this.route === 'labslist'"/>
        <ManageReservations v-if="this.route === 'managereservations'"/>
        <MyReservations v-if="this.route === 'myreservations'"/>
        <ReservationRequest v-if="this.route === 'reservationrequest'"/>
        <Footer v-if="this.route !== 'login'"/>
    </div>
</template>

<style>
</style>
