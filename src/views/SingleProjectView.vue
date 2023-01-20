<script>
import axios from 'axios'
export default {
    name: 'SingleProjectView',
    data() {
        return {
            post: null,
            loading: true,
            api_base_url: 'http://127.0.0.1:8000'
        }
    },
    mounted() {
        const url = this.api_base_url + '/api/projects/' + this.$route.params.slug
        axios.get(url)
            .then(response => {
                if (response.data.success) {
                    this.post = response.data.results
                    this.loading = false
                } else {
                    /* error404  (metodo this.$router.push) */
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
        <img class="img-fluid w-100" :src="api_base_url + '/storage/' + project.thumb" :alt="project.title">
        <div class="container">
            <h2>
                {{ project.title }}
            </h2>
            <div class="content">
                {{ project.language }}
            </div>
        </div>
    </div>
</template>


<style lang="scss" scoped>

</style>