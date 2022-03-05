<template>
    <div>
        <!-- TODO: Crear componente GitHubRepos -->
            <div v-for="(item, index) in repolist" :key="index">          
                <p><a :href="item.html_url" target="_blank">{{ item.full_name }}</a> 
                <br>
                La descripción de este proyecto es: {{ item.description }} 
                <br>
                Cantidad de forks: {{ item.forks_count }} <img src="imagen_fork.png" style="width: 8px"> </p>
                
            </div>
    </div>
</template>

<script>
export default {
    name: 'GitHubRepos',
    props: [
        // TODO: crear parámetro 'repolist'
        'repolist'
    ],
}
</script>

<style scoped>
</style>
