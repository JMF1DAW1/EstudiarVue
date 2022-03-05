<template>
    <div>
        <div>
            <input type="text" placeholder="Introduce un nombre de usuario de GITHUB" v-model="user" @keydown.enter="obtenerUsuario">
        </div>

        <div v-if="userValid">
            <p><img :src="userData.avatar_url"></p>
            <p>El nombre es: {{ userData.login }} </p>
            <a :href="userData.html_url" target="_blank"> Enlace a su url</a>
            <button type="button" @click="obtenerRepositorios"> Repositorios </button>

            <div v-if="mostrarRepositorio">
                <GitHubRepos :repolist="repositorios"></GitHubRepos>
            </div>
        </div>
    </div>
</template>

<script>

// TODO: Importar componente GitHubRepos
import GitHubRepos from "./GitHubRepos.vue"

export default {
    name: 'GitHub',
    components: {
        // TODO: Importar componente GitHubRepos
        GitHubRepos
    },
    data: function() {
        return {
            // TODO: crear variables de datos para el funcionamiento del componente
            user: "",
            userData: {}, 
            userValid: false,
            repositorios: {},
            mostrarRepositorio: true
        }
    },
    methods: {
        obtenerUsuario: function() {

            var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let base64 = require('base-64');

            let url = "https://api.github.com/users/" + this.user;

            let headers = new Headers();

            headers.set('Authorization', 'Basic ' + base64.encode(userAuth + ":" + passAuth));

            fetch(url, {method:'GET',
                    headers: headers,
                })
            .then(res => res.json())
            .then(response => {
                console.log(response);
                this.userData = response;
                this.userValid = true;
            })
        },
        obtenerRepositorios: function() {
             var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let base64 = require('base-64');

            let url = this.userData.repos_url

            let headers = new Headers();

            headers.set('Authorization', 'Basic ' + base64.encode(userAuth + ":" + passAuth));

            fetch(url, {method:'GET',
                    headers: headers,
                })
            .then(res => res.json())
            .then(response => {
                console.log(response);
                this.repositorios = response;
                this.mostrarRepositorio = true;
            })
        }
    }
}
</script>

<style scoped>
</style>
