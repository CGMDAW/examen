<template>
    <div id="app" class="container">
        <h1 class="text-center mb-2 mt-3 text-center">Buscar usuario de GitHub</h1>
        <!-- TODO: Añadir componente GitHub -->

        <git-hub></git-hub>


    </div>
</template>

<script>
// TODO: Importar componente GitHub
import GitHub from './components/GitHub.vue'

export default {
    name: 'App',
    components: {
        // TODO: Añadir componente GitHub
        GitHub
    }
}
</script>

<style>
</style>
