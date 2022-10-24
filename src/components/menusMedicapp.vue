<template>
    <nav class="navbar navbar-expand-lg " :style="`background-color: ${menuColor}`">
        <div class="container-fluid">
            <a class="navbar-brand" href="http://localhost:8080/" title="Pagina Principal">{{marca}}</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent"
                aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item" v-for="menu of menus" :key="menu.id">
                        <a class="nav-link" aria-current="page" :href="menu.url">{{menu.etiqueta}}</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script>
    import axios from "axios";
    export default {
        name: 'menuMarket',
        data(){
            return {
                menuColor: "lightblue",
                marca: "MedicApp",
                menus: [],
                idProducto: 0,
            };
        },
        async created(){
            try {
                const res = await axios.get(`http://localhost:3000/menus`);
                this.menus = res.data;
            }catch(error){
                console.log(error);
            }
        },
        methods: {
        },
        computed: {
        },
        // mounted() {
        // },
        components: {
            
        },
    }
</script>

<style>

</style>