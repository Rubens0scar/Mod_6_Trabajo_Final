<template>
	
    <div>
        <h1>Datos Generales del Paciente</h1>
    </div>
    <div class="container" style="margin: 20px;">
		<div v-if="!isHidden" class="border" style="width: 100%;margin: 0 auto;">
			<div class="row">
				<div class="col"></div>
				<div class="col">
					<label for="codigo" class="form-label" style="margin-top: 20px;">Codigo</label>
					<input type="text" class="form-control" id="codigo" aria-describedby="emailHelp" v-model="codigo" required>
				</div>
				<div class="col"></div>
			</div>
			<div class="row">
				<div class="col">
					<label for="nombres" class="form-label" style="margin-top: 20px;">Nombres</label>
					<input type="text" class="form-control" id="nombres" aria-describedby="emailHelp" v-model="nombres" required>
				</div>
				<div class="col">
					<label for="apellidos" class="form-label" style="margin-top: 20px;">Apellidos</label>
					<input type="text" class="form-control" id="apellidos" aria-describedby="emailHelp" v-model="apellidos" required>
				</div>
				<div class="col">
					<label for="genero" class="form-label" style="margin-top: 20px;">Genero</label>
					<select v-model="form.gene" class="form-select" id="genero" name="genero">
						<option v-for="g in genero" :key="g.id" :value="g.id"> {{g.descripcion}} </option>
					</select>
				</div>
			</div>
			<div class="row">
				<div class="col">
					<label for="edad" class="form-label" style="margin-top: 20px;">Edad</label>
					<input type="text" class="form-control" id="edad" aria-describedby="emailHelp" v-model="edad" required>
				</div>
				<div class="col">
					<label for="correo" class="form-label" style="margin-top: 20px;">Correo Electronico</label>
					<input type="text" class="form-control" id="correo" aria-describedby="emailHelp" v-model="correo" required>
				</div>
				<div class="col">
					<label for="ciudad" class="form-label" style="margin-top: 20px;">Ciudad</label>
					<select v-model="form.ciud" class="form-select" id="ciudad" name="ciudad">
						<option v-for="g in ciudad" :key="g.id" :value="g.id"> {{g.descripcion}} </option>
					</select>
				</div>
			</div>
			<div class="row">
				<div class="col">
					<label for="estado" class="form-label" style="margin-top: 20px;">Estado Civil</label>
					<select v-model="form.esta" class="form-select" id="estado" name="estado">
						<option v-for="g in estado" :key="g.id" :value="g.id"> {{g.descripcion}} </option>
					</select>
				</div>
				<div class="col">
					<label for="telefono" class="form-label" style="margin-top: 20px;">Telefono</label>
					<input type="text" class="form-control" id="telefono" aria-describedby="emailHelp" v-model="telefono">
				</div>
				<div class="col">
					<label for="direccion" class="form-label" style="margin-top: 20px;">Direccion</label>
					<input type="text" class="form-control" id="direccion" aria-describedby="emailHelp" v-model="direccion" required>
				</div>
			</div>
			<div class="row">
				<div class="col"></div>
				<div class="col">
					<label for="educativo" class="form-label" style="margin-top: 20px;">Nivel Educativo</label>
					<select v-model="form.educ" class="form-select" id="educativo" name="educativo">
						<option v-for="g in educativo" :key="g.id" :value="g.id"> {{g.descripcion}} </option>
					</select>
				</div>
				<div class="col">
					<label for="ocupacion" class="form-label" style="margin-top: 20px;">Ocupacion</label>
					<input type="text" class="form-control" id="ocupacion" aria-describedby="emailHelp" v-model="ocupacion" required>
				</div>
				<div class="col"></div>
			</div>
			<div class="row" style="margin-top: 20px;">
				<div class="col">
					<button class="btn btn-primary" v-on:click="guardar()">Guardar</button>
				</div>
				<div class="col">
					<button class="btn btn-primary" v-on:click="cancelar()">Cancelar</button>
				</div>
			</div>
		</div>
		
        
        <div v-on:click="botonCLick" :style="`margin:20px;display: ${display}`">
            <button class="btn btn-primary" v-on:click="isHidden = !isHidden" >Nuevo</button>
        </div>
        <div class="row" style="margin:10px"></div>
		<div class="row">
            <table class="table">
                <thead>
                    <tr>
                        <th scope="col">Identificador</th>
                        <th scope="col">Codigo</th>
                        <th scope="col">Nombres</th>
						<th scope="col">Apellidos</th>
						<th scope="col">Genero</th>
						<th scope="col">Ciudad</th>
                        <th scope="col" colspan="2">opciones</th>
                    </tr>
                </thead>
                <tbody>
                    <template v-for="p in pac" :key="p.id">
                        <tr>
							<td>{{p.id}}</td>
							<td>{{p.codigo}}</td>
							<td>{{p.nombre}}</td>
							<td>{{p.apellido}}</td>
							<td>{{p.genero}}</td>
							<td>{{p.ciudad}}</td>
							<td><img v-on:click="editar(p)" style="cursor:pointer;filter: opacity(0.6) drop-shadow(0 0 0 green); " src="../assets/pencil.svg" alt="Editar" title="Editar"></td>
							<td><img v-on:click="eliminar(p.id)" style="cursor:pointer;filter: opacity(0.6) drop-shadow(0 0 0 red); " src="../assets/x-octagon.svg" alt="Eliminar" title="Eliminar"></td></tr>
                    </template>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
    import axios from "axios";
	
    export default {
        name: 'PacienteDatosGenerales',
        data(){
            return {
				genero: [],
				ciudad: [],
				estado: [],
				educativo: [],
				form: {
					gene: '',
					ciud: '',
					esta: '',
					educ: '',
				},
				codigo: '',
				nombres: '',
				apellidos: '',
				edad: 0,
				correo: '',
				telefono: '',
				direccion: '',
				ocupacion: '',
				isHidden: true,
				display: '',
				pacientes: [],
				pac: [],
				aux: [],
				
                parame: [],
                tipo: [],
                
                
                listaTipo: [],
                selectedTipo: 0,
                
                id: 0,
                op: 'g',
                idMod: 0,
				
            }
        },
        created(){
			this.listar();
            this.listarParametros();
        },
        methods: {
            listar: async function(){
                const res = await axios.get(`http://localhost:3000/paciente`, {
                    // params: {
                    //     'id': this.productos
                    // },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
                this.pacientes = res.data;
				let parametros=[];
                for(let i=1; i<this.pacientes.length; i++){
                    parametros = await axios.get(`http://localhost:3000/parametros`, {
                        params: {
                            'id': this.pacientes[i].id_genero
                        },
                        headers:{
                            'content-type': 'application/json',
                            'accept': 'application/json'
                        }
                    });
                    this.aux = parametros.data;
                    const sexo = this.aux[0].descripcion;
					this.aux=[];
					parametros=[];
					parametros = await axios.get(`http://localhost:3000/parametros`, {
                        params: {
                            'id': this.pacientes[i].id_ciudad
                        },
                        headers:{
                            'content-type': 'application/json',
                            'accept': 'application/json'
                        }
                    });
                    this.aux = parametros.data;
                    const city = this.aux[0].descripcion;
                    this.aux=[];
					
					const datos = {
						"id": this.pacientes[i].id,
						"codigo": this.pacientes[i].codigo,
						"nombre": this.pacientes[i].nombre,
						"apellido": this.pacientes[i].apellidos,
						"genero": sexo,
						"ciudad": city,
						"id_genero": this.pacientes[i].id_genero,
						"id_ciudad": this.pacientes[i].id_ciudad,
						"id_civil": this.pacientes[i].id_estado_civil,
						"id_nivel": this.pacientes[i].id_nivel_educativo,
						"edad": this.pacientes[i].edad,
						"correo": this.pacientes[i].email,
						"telefono": this.pacientes[i].telefono,
						"direccion": this.pacientes[i].direccion,
						"ocupacion": this.pacientes[i].ocupacion,
					};
                    this.pac.push(datos);
                }
				this.listarParametros();
            },
            editar: function(p){
                this.op="m";
				this.id=p.id;
				this.form.gene=p.id_genero;
				this.form.ciud=p.id_ciudad;
				this.form.esta=p.id_civil;
				this.form.educ=p.id_nivel;
				this.codigo=p.codigo;
				this.nombres=p.nombre
				this.apellidos=p.apellido
				this.edad=p.edad;
				this.correo=p.correo;
				this.telefono=p.telefono;
				this.direccion=p.direccion;
				this.ocupacion=p.ocupacion;
                this.isHidden=false;
				this.display = 'none';
            },
            eliminar: function(id){
                var opcion = confirm("Esta seguro de eliminaro el registro?");
                if (opcion == true) {
                    const a = axios.delete("http://localhost:3000/paciente/"+id);
					this.pac = [];
                }
                this.listar();
            },
            cancelar: function(){
                this.pac=[];
				this.limpiar();
                this.isHidden=true;
                this.descripcion='';
                this.display = '';
                this.listarParametros();
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
				this.limpiar();
                this.display = 'none';
            },
            guardar: function(){
                if(this.op=="g"){
                    axios.post(`http://localhost:3000/paciente`, {
							codigo: this.codigo,
							nombre: this.nombres,
							apellidos: this.apellidos,
							id_genero: this.form.gene,
							email: this.correo,
							id_ciudad: this.form.ciud,
							edad: this.edad,
							id_estado_civil: this.form.esta,
							telefono: this.telefono,
							direccion: this.direccion,
							id_nivel_educativo: this.form.educ,
							ocupacion: this.ocupacion
                        }).then(response => {
							this.pac = [];
                            this.isHidden=true;
                            this.descripcion='';
                            this.display = '';
                            this.listar();
                        }).catch(error => {
                            console.log(error);
                    });
                }else{
					const params = {
						codigo: this.codigo,
						nombre: this.nombres,
						apellidos: this.apellidos,
						id_genero: this.form.gene,
						email: this.correo,
						id_ciudad: this.form.ciud,
						edad: this.edad,
						id_estado_civil: this.form.esta,
						telefono: this.telefono,
						direccion: this.direccion,
						id_nivel_educativo: this.form.educ,
						ocupacion: this.ocupacion
					};
                    axios.put(`http://localhost:3000/paciente/${this.id}`, params).then((response) => {
						this.pac = [];
                        const product = response.data;
                        this.isHidden=true;
                        this.descripcion='';
                        this.display = '';
                        this.listar();
                    });
                    this.op="g";
                }
				
				this.limpiar();
            },
			limpiar: function(){
				this.form.gene='';
				this.form.ciud='';
				this.form.esta='';
				this.form.educ='';
				this.codigo='';
				this.nombres='';
				this.apellidos='';
				this.edad=0;
				this.correo='';
				this.telefono='';
				this.direccion='';
				this.ocupacion='';
			},
			listarParametros: async function(){
				const gen = await axios.get(`http://localhost:3000/parametros`, {
                    params: {
						'tipo': 1
                    },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
                this.genero = gen.data;
				const ciu = await axios.get(`http://localhost:3000/parametros`, {
                    params: {
						'tipo': 6
                    },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
				this.ciudad = ciu.data;
				const est = await axios.get(`http://localhost:3000/parametros`, {
                    params: {
						'tipo': 5
                    },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
				this.estado = est.data;
				const edu = await axios.get(`http://localhost:3000/parametros`, {
                    params: {
						'tipo': 7
                    },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
				this.educativo = edu.data;
			},
        },
        computed: {
			selectedGenero() {
                return this.form.gene?.id;
            },
			selectedCiudad() {
                return this.form.ciud?.id;
            },
			selectedEstadoCivil() {
                return this.form.esta?.id;
            },
			selectedNivelEducativo() {
                return this.form.educ?.id;
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