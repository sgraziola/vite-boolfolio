<script>
import ProjectCard from './components/ProjectCard.vue'
export default {
    components: {
        ProjectCard,
    },

}
</script>

<template>
    <section class="vue-home">
        <div class="container">
            <h1>Projects</h1>

            <ProjectCard />
        </div>
    </section>
</template>

<style lang="scss">
@use './styles/general.scss';
</style>
