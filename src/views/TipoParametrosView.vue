<template>
    <div>
        <h1>Tipos de Parametros</h1>
    </div>
    <div class="container">
        <div class="row">
            <table class="table">
                <thead>
                    <tr>
                        <th scope="col">Identificador</th>
                        <th scope="col">Descripcion</th>
                        <th scope="col" colspan="2">opciones</th>
                    </tr>
                </thead>
                <tbody>
                    <template v-for="tipo in tipos" :key="tipo.id">
                        <tr><td>{{tipo.id}}</td><td>{{tipo.descripcion}}</td><td><img v-on:click="editar(tipo.id,tipo.descripcion)" style="cursor:pointer;filter: opacity(0.6) drop-shadow(0 0 0 green); " src="../assets/pencil.svg" alt="Editar" title="Editar"></td><td><img v-on:click="eliminar(tipo.id)" style="cursor:pointer;filter: opacity(0.6) drop-shadow(0 0 0 red); " src="../assets/x-octagon.svg" alt="Eliminar" title="Eliminar"></td></tr>
                    </template>
                </tbody>
            </table>
        </div>
        <button class="btn btn-primary" v-on:click="isHidden = !isHidden">Nuevo</button>
        <div class="row">
            <div v-if="!isHidden" class="border" style="width: 40%;margin: 0 auto;margin-top: 20px;">
                <div class="mb-3">
                    <label for="descripcion" class="form-label" style="margin-top: 20px;">Descripcion</label>
                    <input type="text" class="form-control" id="descripcion" aria-describedby="emailHelp" v-model="descripcion" required>
                </div>
                <div class="row" style="margin-bottom: 20px;">
                    <div class="col">
                        <button class="btn btn-primary" v-on:click="guardar()">Guardar</button>
                    </div>
                    <div class="col">
                        <button class="btn btn-primary" v-on:click="cancelar()">Cancelar</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
	
    import axios from "axios";
    export default {
        name: 'TipoParametros',
        data(){
            return {
                tipos: [],
                isHidden: true,
                descripcion: '',
                id: 0,
                op: 'g'
            }
        },
        created(){
            this.listar();
        },
        methods: {
            guardar: function(){
                if(this.op=="g"){
                    axios.post(`http://localhost:3000/tipo_parametro`, {descripcion: this.descripcion
                        }).then(response => {
                            this.isHidden=true;
                            this.descripcion='';
                            this.listar();
                        }).catch(error => {
                            console.log(error);
                    });
                }else{
                    const params = {
                        descripcion: this.descripcion
                    };
                    axios.put(`http://localhost:3000/tipo_parametro/${this.id}`, params).then((response) => {
                        const product = response.data;
                        this.isHidden=true;
                        this.descripcion='';
                        this.listar();
                    });

                    this.op="g";
                }
            },

            listar: async function(){
                const res = await axios.get(`http://localhost:3000/tipo_parametro`, {
                    // params: {
                    //     'id': this.productos
                    // },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
                this.tipos = res.data;
            },
            cancelar: function(){
                this.isHidden=true;
                this.descripcion='';
                this.listar();
            },
            editar: function(id,texto){
                //alert(id);
                this.op="m";
                this.descripcion=texto;
                this.id=id;
                this.isHidden=false;
            },
            eliminar: function(id){
                var opcion = confirm("Esta seguro de eliminaro el registro?");
                if (opcion == true) {
                    const a = axios.delete("http://localhost:3000/tipo_parametro/"+id);
                }
                this.listar();
            },
        },
        computed: {
        },
        //mounted() {
        //},
        components: {
        },
    }
</script>

<style>
</style>