<template>
	<div>
        <h1>Parametros</h1>
    </div>
    <div class="container">
        <div class="row" style="margin: 20px;">
            <div v-if="!isHidden" class="border" style="width: 40%;margin: 0 auto;">
                <div class="mb-3">
                    <label for="descripcion" class="form-label" style="margin-top: 20px;">Descripcion</label>
                    <input type="text" class="form-control" id="descripcion" aria-describedby="emailHelp" v-model="descripcion" required>
                </div>
                <div class="mb-3">
                    <label for="tipo" class="form-label" style="margin-top: 20px;">Tipo de Parametro</label>
                    <select v-model="form.item" class="form-select" id="tipos" name="tipos">
                        <option 
                            v-for="tipo in listaTipo"
                            :key="tipo.id"
                            :value="tipo.id"
                        >
                            {{tipo.descripcion}} 
                        </option> 
                    </select>
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
        <div v-on:click="botonCLick" :style="`margin:20px;display: ${display}`">
            <button class="btn btn-primary" v-on:click="isHidden = !isHidden" >Nuevo</button>
        </div>
        <div class="row">
            <table class="table">
                <thead>
                    <tr>
                        <th scope="col">Identificador</th>
                        <th scope="col">Descripcion</th>
                        <th scope="col">Tipo</th>
                        <th scope="col" colspan="2">opciones</th>
                    </tr>
                </thead>
                <tbody>
                    <template v-for="p in parame" :key="p.id">
                        <tr><td>{{p.id}}</td><td>{{p.descripcion}}</td><td>{{p.tipo}}</td><td><img v-on:click="editar(p.id,p.idTipo,p.descripcion)" style="cursor:pointer;filter: opacity(0.6) drop-shadow(0 0 0 green); " src="../assets/pencil.svg" alt="Editar" title="Editar"></td><td><img v-on:click="eliminar(p.id)" style="cursor:pointer;filter: opacity(0.6) drop-shadow(0 0 0 red); " src="../assets/x-octagon.svg" alt="Eliminar" title="Eliminar"></td></tr>
                    </template>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
    import axios from "axios";
    export default {
        name: 'ParametrosGenerales',
        data(){
            return {
                parame: [],
                tipo: [],
                parametros: [],
                isHidden: true,
                listaTipo: [],
                selectedTipo: 0,
                display: '',
                id: 0,
                op: 'g',
                idMod: 0,
				form: {item: ''}
            }
        },
        created(){
            this.listar();
            this.listarTipos();
        },
        methods: {
            listar: async function(){
                
                const res = await axios.get(`http://localhost:3000/parametros`, {
                    // params: {
                    //     'id': this.productos
                    // },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
                this.parametros = res.data;
                for(var i=0; i<this.parametros.length; i++){
                    const res1 = await axios.get(`http://localhost:3000/tipo_parametro`, {
                        params: {
                            'id': this.parametros[i].tipo
                        },
                        headers:{
                            'content-type': 'application/json',
                            'accept': 'application/json'
                        }
                    });
                    this.tipo = res1.data;
                    const des = this.tipo[0].descripcion;
                    const idTipo = this.tipo[0].id;
                    this.para ={
                        "id": this.parametros[i].id,
                        "descripcion": this.parametros[i].descripcion,
                        "idTipo": idTipo,
                        "tipo": des,
                    };
                    this.parame.push(this.para);
                }
                this.listarTipos();
            },
            editar: function(id,idTipo,texto){
                this.op="m";
                this.descripcion=texto;
                this.id=id;
                this.idMod = idTipo;
				this.form.item = idTipo;
                this.isHidden=false;
				this.display = 'none';
            },
            eliminar: function(id){
                var opcion = confirm("Esta seguro de eliminaro el registro?");
                if (opcion == true) {
                    const a = axios.delete("http://localhost:3000/parametros/"+id);
					this.parame = [];
                }
                this.listar();
            },
            cancelar: function(){
                //alert(this.selectedTipo);
                this.isHidden=true;
                this.descripcion='';
                this.display = '';
                this.listarTipos();
                this.listar();
            },
            listarTipos: async function(){
                const res1 = await axios.get(`http://localhost:3000/tipo_parametro`, {
                // params: {
                //         'id': this.parametros[i].tipo
                //     },
                headers:{
                    'content-type': 'application/json',
                    'accept': 'application/json'
                    }
                });
                this.listaTipo = res1.data;
            },
            botonCLick: function(){
                this.display = 'none';
            },
            guardar: function(){
                if(this.op=="g"){
                    axios.post(`http://localhost:3000/parametros`, {descripcion: this.descripcion, tipo: this.selectedTipo
                        }).then(response => {
							this.parame = [];
                            this.isHidden=true;
                            this.descripcion='';
                            this.display = '';
                            this.listar();
                        }).catch(error => {
                            console.log(error);
                    });
                }else{
                    const params = {
                        descripcion: this.descripcion,
                        tipo: this.form.item
                    };
                    axios.put(`http://localhost:3000/parametros/${this.id}`, params).then((response) => {
						this.parame = [];
                        const product = response.data;
                        this.isHidden=true;
                        this.descripcion='';
                        this.display = '';
                        this.listar();
                    });
                    this.op="g";
                }
            },
        },
        computed: {
			selectedTipoId() {
                return this.form.item?.id;
            }
        },
        //mounted() {
        //},
        components: {
			
        },
    }
</script>

<style>
</style>