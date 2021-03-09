<template>
    <div>
        <!-- TODO: Crear componente GitHub -->

        <div class="form-group pt-4 pb-2">
            <input type="text" class="form-control" id="searchBar" v-model="user" aria-describedby="searchBar" @keydown.enter="obtenerUsuario" :readonly="disabled">
        </div>

        <div v-if="isError" class="alert alert-warning" role="alert">
            No se ha encontrado el usuario especificado
        </div>

        <div class="row justify-content-center" v-if="validUser">

            <div class="col-12 col-md-6">
                <div class="card bg-light align-items-center">
                    <img :src="userData.avatar_url" class="img-responsive rounded-circle mt-4 shadow" width="180px" alt="avatar">
                    <div class="card-body">
                        <h5 class="card-title text-center pb-2">{{userData.login}}</h5>
                        <button class="btn btn-primary" @click="obtenerRepos">Mostrar Repositorios</button>
                    </div>
                </div>
            </div>
            

            <div class="col-12 col-md-6 pl-2 mb-4">
                <git-hub-repos v-if="showRepos" :repolist="repos"></git-hub-repos>
            </div>
            
        </div>
        

        

    </div>
</template>

<script>
// Librería axios para realizar peticiones AJAX: https://github.com/axios/axios
import axios from 'axios'
// Importar datos de usuario de GiHub para hacer llamadas autenticadas a la API
import userAuthData from '../../usuario.json';

// TODO: Importar componente GitHubRepos
import GitHubRepos from './GitHubRepos.vue';

export default {
    name: 'GitHub',
    components: {
        // TODO: Importar componente GitHubRepos
        GitHubRepos
    },
   data: function() {
        return {
            user: '',
            userData: {},
            repos: [],
            isError : false,
            disabled: false,
            showRepos: false,
            validUser: false
        }
    },
    methods: {
        obtenerUsuario: function() {
            // Función para obtener los datos de usuario de la API de GitHub
            
          

            // Resetear variable de error
            this.isError = false;

            // No mostrar lista de repositorios
            this.showRepos = false;

            // Crear URL del usuario
            var url = `https://api.github.com/users/${this.user}`;

            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub
            // ¡OJO! userAuth no tiene nada que ver con la variable local 'this.user'
            var userAuth = userAuthData.user;
            var passAuth = userAuthData.pass;

            // Lanzar petición AJAX con Axios
            axios
                .get(url, {
                    auth: {
                        username: userAuth,
                        password: passAuth
                    }
                })
                .then(response => {
                    // La petición es válida y la respuesta contiene los datos del usuario.
                    // El parámetro 'response' contiene el resultado de la petición Ajax.
                    // Los datos se hayan en el campo 'response.data'
                    // El objeto con los datos del usuario se almacena en la variable 'userData'
                    this.userData = response.data;
                    console.log(this.userData)
                    // Indicamos que el usuario es válido
                    this.validUser = true;
                      // Deshabilitar campo de texto
                    this.disabled = true;
                })
                .catch(error => {
                    // La petición se ha encontado con un error (posiblemente, usuario inexistente)
                    error;
                    // Indicamos que no se han cargado correctamente los datos de usuario
                    this.validUser = false;

                    // Indicar que hay un error
                    this.isError = true;
                })

            // Al terminar, volver a habilitar el campo de texto
            this.disabled = false;
        },
        obtenerRepos: function() {
            // Función para obtener los repositorios del usuario desde la API de GítHub
            // La URL de los repositorios de usuario se puede obtener a través del campo 'repos_url' de los datos del usuario
            var url = this.userData.repos_url;

            // Obtener datos de autenticación de usuario para hacer peticiones
            // autenticadas a la API de GitHub
            // ¡OJO! userAuth no tiene nada que ver con la variable local 'this.user'
            var userAuth = userAuthData.user;
            var passAuth = userAuthData.pass;

            // Lanzar petición AJAX con Axios
            axios
                .get(url, {
                    auth: {
                        username: userAuth,
                        password: passAuth
                    }
                })
                .then(response => {
                    // La petición es válida. Almacenamos los datos de la respuesta en la variable local 'repos'
                    this.repos = response.data;

                    // Mostrar lista de repositorios
                    this.showRepos = true;
                })
                .catch(error => {
                    // La petición no es válida. Indicar que no se han cargado correctamente los repositorios
                    error;
                    this.validUser = false;
                    this.showRepos = false;

                    // Indicar que hay un error
                    this.isError = true;
                })
        }
    }
}
</script>

<style scoped>
</style>
