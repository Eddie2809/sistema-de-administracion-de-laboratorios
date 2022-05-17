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
    
*/
    import Modal from "../components/Modal.vue"
export default {
    
    data() {
        return {
            input: '', //entrada de busqueda
            isModalVisible: false,
            nombre: '', //nombre usado en "agregar laboratorio"
            nuevoEncargado: '', //ID del nuevo encargado usado en "asignar encargado"
            modalVersion: 0, //Variable para identificar que ventana emergente mostrar
            idLabCambio: 0, //ID del laboratorio sobre el que se aplica modificar, eliminar o asignar
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
        setidLabCambio(newidLabCambio){
            this.idLabCambio = newidLabCambio;
        }
    },
    computed: {
        filteredItems() {
            return this.lablist.filter((i) =>{
                return i.nombre_laboratorio.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.nombre_encargado.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.apellido_encargado.toLowerCase().indexOf(this.input.toLowerCase()) != -1 || i.correo_encargado.toLowerCase().indexOf(this.input.toLowerCase()) != -1;
            });
        }
    },
    components: {
        Modal
    },
    props: ['lablist', 'getLabs', "assignLabManager", "deleteLab", "modifyLab", "addNewLab"],
    mounted() {
        this.getLabs()
    }
}
</script>

<template>
    <div> 
        <h2 class="subtitle">Laboratorios</h2>
        <div class="add-search">
            <button @click="setModalVersion(1); showModal();" id="agg" href="#">Agregar laboratorio</button>
            <p>Buscar:</p>
            <input type="text" id="write" placeholder="Por nombre, encargado o correo" v-model="input">

        </div>

        <Modal v-show="isModalVisible" @close="closeModal" v-if="modalVersion === 1">
            <template v-slot:header>
                Agregar Laboratorio
            </template>
            <template v-slot:body>
                <div>
                    <p>Nombre del laboratorio:</p>
                    <input id="border-r5" type="text" v-model="nombre" placeholder="Laboratorio de ingenieria">
                </div>
                <div>
                    <p>ID del encargado:</p>
                    <input id="border-r5" type="text" v-model="id_encargado" placeholder="1"> 
                </div>
            </template>
            <template v-slot:footer>
                <button id="agg" @click="addNewLab(this.nombre, this.id_encargado); closeModal(); getLabs();">Agregar</button>
            </template>
        </Modal>

        <Modal v-show="isModalVisible" @close="closeModal" v-if="modalVersion === 2">
            <template v-slot:header>
                Asignar Encargado
            </template>
            <template v-slot:body>
                <div>
                    <p>ID del nuevo encargado:</p>
                    <input id="border-r5" type="text" v-model="nuevoEncargado" placeholder="1"> 
                </div>
            </template>
            <template v-slot:footer>
                <button id="agg" @click="assignLabManager(this.idLabCambio, this.nuevoEncargado); closeModal(); getLabs();">Asignar</button>
            </template>
        </Modal>

        <Modal v-show="isModalVisible" @close="closeModal" v-if="modalVersion === 3">
            <template v-slot:header>
                Modificar Laboratorio
            </template>
            <template v-slot:body>
                <div>
                    <p>Nuevo nombre:</p>
                    <input id="border-r5" type="text" v-model="nuevoNombre" placeholder="Laboratorio nuevo"> 
                </div>
            </template>
            <template v-slot:footer>
                <button id="agg" @click="modifyLab(this.idLabCambio, this.nuevoNombre, 1); closeModal(); getLabs();">Modificar</button>
            </template>
        </Modal>

        <Modal v-show="isModalVisible" @close="closeModal" v-if="modalVersion === 4">
            <template v-slot:header>
                Eliminar Laboratorio
            </template>
            <template v-slot:body>
                <div>
                    <p>¿Seguro de que desea eliminar el laboratorio?</p>
                </div>
            </template>
            <template v-slot:footer>
                <button id="agg" @click="deleteLab(this.idLabCambio); closeModal(); getLabs();">Eliminar</button>
            </template>
        </Modal>

        <table>
            <tr>
                <th>Nombre</th>
                <th>Encargado</th>
                <th>Correo</th>
                <th>Opciones</th>
            </tr>
            <tr v-for="item in filteredItems" :key="item.id_laboratorio">
                <td>{{item.nombre_laboratorio}}</td>
                <td>{{item.nombre_encargado + " " + item.apellido_encargado}}</td>
                <td>{{item.correo_encargado}}</td>
                <td>
                    <button @click="setModalVersion(2); setidLabCambio(item.id_laboratorio); showModal();" class="asignar">Asignar encargado</button>
                    <button @click="setModalVersion(3); setidLabCambio(item.id_laboratorio); showModal();" class="modificar">Modificar</button> 
                    <button @click="setModalVersion(4); setidLabCambio(item.id_laboratorio); showModal()" class="eliminar">Eliminar</button>
                </td>
            </tr>
            
        </table>
    </div>
    
</template>