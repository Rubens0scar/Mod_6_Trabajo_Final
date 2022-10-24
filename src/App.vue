<template>
	<menuMed></menuMed>
	<router-view/>
	<div class="container">
		<div class="row">
			<div class="col">
				<h5>Buscar Paciente Nombres</h5>
				<div class="mb-3"><input type="text" class="form-control" id="texto" aria-describedby="emailHelp" v-model="texto" required></div>
				<button class="btn btn-primary" title="Buscar" v-on:click="buscar()">Buscar</button>
			</div>
			<div class="col">
				<h5>Buscar Paciente Apellidos</h5>
				<div class="mb-3"><input type="text" class="form-control" id="apellido" aria-describedby="emailHelp" v-model="apellido" required></div>
				<button class="btn btn-primary" title="Buscar" v-on:click="buscarAp()">Buscar</button>
			</div>
			<div class="col">
				<h5>&nbsp;</h5>
				<button class="btn btn-primary" title="Buscar" v-on:click="limpiar()">Limpiar</button>
			</div>
		</div>
		<div class="row" style="margin:15px;">
			<div class="col">
				<table class="table">
                <thead>
                    <tr>
                        <th scope="col">Identificador</th>
                        <th scope="col">Codigo</th>
						<th scope="col">Nombres</th>
						<th scope="col">Apellidos</th>
						<th scope="col">Telefono</th>
                        <th scope="col">Opcion</th>
                    </tr>
                </thead>
                <tbody>
                    <template v-for="p in pac" :key="p.id">
                        <tr>
							<td>{{p.id}}</td><td>{{p.codigo}}</td><td>{{p.nombre}}</td><td>{{p.apellidos}}</td><td>{{p.telefono}}</td>
							<td><button class="btn btn-primary" title="En construccion">Consulta Medica</button></td>
						</tr>
                    </template>
                </tbody>
            </table>
			</div>
		</div>
	</div>
</template>
<script>
	import menuMed from '@/components/menusMedicapp.vue';
    import axios from "axios";
    export default {
        name: 'PaginaPrincipal',
        data(){
            return {
				paciente: [],
				pac: [],
				texto: '',
				apellido: '',
            };
        },
        created(){
            this.listar();
        },
        methods: {
			listar: async function(){
				try {
					const res = await axios.get(`http://localhost:3000/paciente`);
					this.paciente = res.data;
					let data=[];
					for(let i=1; i<this.paciente.length; i++){
						this.pac.push(this.paciente[i]);
					}
				}catch(error){
					console.log(error);
				}
			},
			buscar: async function(){
				this.pac = [];
				const res = await axios.get(`http://localhost:3000/paciente`, {
                    params: {
						'estado': 1
                    },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
				let resultado = res.data;
				
				this.pac = resultado.filter((item) =>
					item.nombre.includes(this.texto)
					);
			},
			buscarAp: async function(){
				this.pac = [];
				const res = await axios.get(`http://localhost:3000/paciente`, {
                    params: {
						'estado': 1
                    },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
				let resultado = res.data;
				
				this.pac = resultado.filter((item) =>
					item.apellidos.includes(this.apellido)
					);
			},
			limpiar: function(){
				this.pac=[];
				this.paciente=[];
				this.texto='';
				this.apellido='';
				this.listar();
			}
        },
        computed: {
        },
        // mounted() {
        // },
        components: {
            menuMed
        },
    }
</script>
<style>
</style>
