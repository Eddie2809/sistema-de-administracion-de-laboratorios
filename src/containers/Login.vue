<script>
    /*
        Asignado a: Erick 

        Notas:
        -Las imágenes necesarias las puedes importar de assets

        Funciones útiles:
        -evaluateCredentials
    */

    export default {
        data(){
            return {
                username: "",
                password: ""
            }
        },
        props: ['evaluateCredentials','changeRoute']
    }
</script>

<template>
    <div class="inicio"></div>
    <img id="uni" src="../assets/banner-unicaribe.png" alt="imagen uni">
    <div class="square"></div>
    <img id="sal" src="../assets/logo-con-subtitulo-oscuro.png" alt="imagen sal">
    <p id="ingresar">Ingresar</p>
    <p id="usuario">Usuario</p>
    <p id="contra">Contraseña</p>
    <form action="">
        <input type="text" name="write1" id="write1" v-model="username">
        <input type="password" name="write2" id="write2" v-model="password">
    </form>

    <button @click="this.evaluateCredentials(this.username,this.password)" id="enviar" autofocus>Enviar</button>

    <div class="rectangle"></div>
    <a id="regresar" href="#" @click="this.changeRoute('home')" >Regresar a página principal</a>
    <a id="unicaribe" href="https://www.unicaribe.mx">Visitar unicaribe.mx</a>
</template>