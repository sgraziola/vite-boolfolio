<script>
//import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios';

export default {
    components: {

    },
    data() {
        return {
            projects: null,
            base_api_url: 'http://127.0.0.1:8000',
            loading: true,
            error: null,
        }
    },
    methods: {
        getProjects(url) {
            //ajax call
            axios
                .get(url)
                .then(response => {
                    console.log(response.data.results);
                    this.projects = response.data.results;
                    this.loading = false;

                })
                .catch(error => {
                    console.error(error.message);
                    this.error = error.message;
                    this.loading = false;

                })

        }
    },
    mounted() {
        this.getProjects(this.base_api_url + '/api/projects');
    }
}
</script>

<template>
    <section class="vue-home">
        <div class="container">
            <h1>Projects</h1>
            <!-- <div class="row">
                <div class="col-12 d-flex flex-column justify-content-center align-items-center vh-100">
                   
                </div>
            </div> -->
        </div>
    </section>
</template>

<style lang="scss">
@use './styles/general.scss';
</style>
