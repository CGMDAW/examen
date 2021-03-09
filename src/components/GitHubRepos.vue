<template>
        <!-- TODO: Crear componente GitHubRepos -->
        <ul class="list-group">
            <li  v-for="(item, index) in repolist" :key="index" 
                v-b-tooltip.hover.bottom 
                :title="item.description" 
                class="justify-content-between list-group-item list-group-item-action d-flex align-items-center"
                aria-current="true"
            >
                    <a :href="item.html_url" class="ml-3 text-decoration-none text-dark stretched-link">{{item.full_name}}</a>
                    <span class="badge badge-primary mr-3 flex-shrink-1">{{item.stargazers_count}} ★</span>
            </li>
        </ul>
        
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
