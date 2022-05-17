<script>
/* 
    Asignado a: Eduardo y Erick

    A tomar en cuenta:

    -Reservaciones es casi la misma pantalla que 'Administrar reservaciones', ponerse de acuerdo para reciclar
    componentes.
    -Al eliminar usuario o laboratorio siempre abrir una ventana de confirmación
    -Cargar componentes de la barra de navegación izquierda dentro de este mismo componente

    Funciones útiles:
    -addNewUser
    -modifyUser
    -getUsers
    -deleteUser
*/
import Modal from "../components/Modal.vue"
export default {
    
    data() {
        return {
            input: '', //entrada de busqueda
            isModalVisible: false,
            aggNombre: "",
            aggApellido: "",
            aggCorreo: "",
            aggTipo: 0,

            nuevoNombre: "",
            nuevoApellido: "",
            nuevoCorreo: "",
            nuevaContraseña: "",
            nuevoUsrName: "",
            nuevoTipo: undefined, 

            modalVersion: 0, //Variable para identificar que ventana emergente mostrar
            idUserCambio: 0, //ID del laboratorio sobre el que se aplica modificar, eliminar o asignar

            nuevoNombre: "",    //Nuevo nombre usado en "modificar laboratorio"
        }
    },
    methods:{
        showModal() {
            this.isModalVisible = true;
        },
        closeModal(){
            this.isModalVisible = false;
        },
        setModalVersion(newModalVersion){
            this.modalVersion = newModalVersion;
        },
        setidUserCambio(newidUserCambio){
            this.idUserCambio = newidUserCambio;
        }
    },
    computed: {
        filteredItems() {
            return this.userlist.filter((i) =>{
                return i.nombre.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.apellido.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.correo.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.tipo.toLowerCase().indexOf(this.input.toLowerCase()) != -1 ;
            });
        }
    },
    components: {
        Modal
    },
    props: ['userlist', 'getUsers', "addNewUser", "modifyUser", "deleteUser"],
    mounted() {
        this.getUsers()
    }
}
    
</script>

<template>
    <div> 
        <h2 class="subtitle">Docentes</h2>
        <div class="add-search">
            <button @click="setModalVersion(1); showModal();" id="agg" href="#">Agregar docente</button>
            <p>Buscar:</p>
            <input type="text" placeholder="Por nombre, correo o tipo de usuario" v-model="input">

        </div>

        <Modal v-show="isModalVisible" @close="closeModal" v-if="modalVersion === 1">
            <template v-slot:header>
                Agregar Docente
            </template>
            <template v-slot:body>
                <div>
                    <p>Nombres del Docente:</p>
                    <input id="border-r5" type="text" v-model="aggNombre" placeholder="Juan">
                </div>
                <div>
                    <p>Apelllidos del Docente:</p>
                    <input id="border-r5" type="text" v-model="aggApellido" placeholder="Perez">
                </div>
                <div>
                    <p>Correo:</p>
                    <input id="border-r5" type="text" v-model="aggCorreo" placeholder="jperez@gmail.com">
                </div>
                <div>
                    <p>tipo de usuario:</p>
                    <input id="border-r5" type="text" v-model="aggTipo" placeholder="1:admin, 2:encargado, 3:docente"> 
                </div>
            </template>
            <template v-slot:footer>
                <button id="agg" @click="addNewUser(this.aggNombre, this.aggApellido, this.aggCorreo, this.aggTipo); closeModal(); getUsers();">Agregar</button>
            </template>
        </Modal>

        <Modal v-show="isModalVisible" @close="closeModal" v-if="modalVersion === 2">
            <template v-slot:header>
                Modificar Docente
            </template>
            <template v-slot:body>
                <div>
                    <p>Nuevo nombre:</p>
                    <input id="border-r5" type="text" v-model="nuevoNombre"> 
                </div>
                <div>
                    <p>Nuevo apellido:</p>
                    <input id="border-r5" type="text" v-model="nuevoApellido"> 
                </div>
                <div>
                    <p>Nuevo correo:</p>
                    <input id="border-r5" type="text" v-model="nuevoCorreo"> 
                </div>
                <div>
                    <p>Nueva contraseña:</p>
                    <input id="border-r5" type="text" v-model="nuevaContraseña"> 
                </div>
                <div>
                    <p>Nuevo nombre de usuario:</p>
                    <input id="border-r5" type="text" v-model="nuevoUsrName"> 
                </div>
                <div>
                    <p>Nuevo tipo de usuario:</p>
                    <input id="border-r5" type="text" v-model="nuevoTipo" placeholder="1:admin, 2:encargado, 3:docente"> 
                </div>
            </template>
            <template v-slot:footer>
                <button id="agg" @click="modifyUser(this.idUserCambio, this.nuevoNombre, this.nuevoApellido, this.nuevoCorreo, this.nuevaContraseña, this.nuevoUsrName, this.nuevoTipo); closeModal(); getUsers();">Modificar</button>
            </template>
        </Modal>

        <Modal v-show="isModalVisible" @close="closeModal" v-if="modalVersion === 3">
            <template v-slot:header>
                Eliminar Usuario
            </template>
            <template v-slot:body>
                <div>
                    <p>¿Seguro de que desea eliminar este usuario?</p>
                </div>
            </template>
            <template v-slot:footer>
                <button id="agg" @click="deleteUser(this.idUserCambio); closeModal(); getUsers();">Eliminar</button>
            </template>
        </Modal>

        <table>
            <tr>
                <th>Nombre</th>
                <th>Correo</th>
                <th>Tipo de Usuario</th>
                <th>Opciones</th>
            </tr>
            <tr v-for="item in filteredItems" :key="item.id_encargado">
                <td>{{item.nombre + " " + item.apellido}}</td>
                <td>{{item.correo}}</td>
                <td>{{item.tipo}}</td>
                <td>
                    <button @click="setModalVersion(2); setidUserCambio(item.id); showModal();" class="modificar">Modificar</button> 
                    <button @click="setModalVersion(3); setidUserCambio(item.id); showModal()" class="eliminar">Eliminar</button>
                </td>
            </tr>
            
        </table>
    </div>
    
</template>