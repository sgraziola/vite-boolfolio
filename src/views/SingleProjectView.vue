<script>
import axios from 'axios'
export default {
    name: 'SingleProjectView',
    data() {
        return {
            project: null,
            loading: true,
            api_base_url: 'http://127.0.0.1:8000'
        }
    },
    mounted() {
        const url = this.api_base_url + '/api/projects/' + this.$route.params.slug
        axios.get(url)
            .then(response => {
                if (response.data.success) {
                    this.project = response.data.results
                    this.loading = false
                } else {
                    /* error404 */
                    this.$router.push({ name: 'not-found' })
                }

            }).catch(error => {
                console.log(error)
            })
    }
}

</script>

<template>
    <h1></h1>
    <div class="single-project" v-if="project">
        <img v-if="project.thumb" class="img-fluid w-100" :src="api_base_url + '/storage/' + project.thumb"
            :alt="project.title">
        <img v-else src="/img/noimg.jpg" alt="">
        <div class="container">
            <h2>
                {{ project.title }}
            </h2>
            <div class="content">
                {{ project.language }}
            </div>
            <div class="card-title">
                <strong>Type: </strong>
                <span v-if="project.type">
                    {{ project.type.name }}
                </span>
                <span v-else>No type selected</span>
            </div>
            <div class="card-title">
                <strong>Technologies: </strong>
                <template v-if="project.technologies.length > 0">
                    <span v-for="technology in project.technologies">
                        #{{ technology.name }}
                    </span>
                </template>
                <template v-else>
                    <span>No technologies.</span>
                </template>
            </div>
        </div>
    </div>
</template>


<style lang="scss" scoped>

</style>