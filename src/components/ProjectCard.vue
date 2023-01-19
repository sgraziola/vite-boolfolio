
<script>
import axios from 'axios';

export default {
    name: 'ProjectCard',
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
                    this.projects = response.data.results.data;
                    this.loading = false;

                })
                .catch(error => {
                    console.error(error.message);
                    this.error = error.message;
                    this.loading = false;

                })

        },
        getImagePath(path) {
            console.log(path);
            if (path) {
                return this.base_api_url + '/storage/' + path
            }
            return '/img/noimg.jpg'
        },
    },
    mounted() {
        this.getProjects(this.base_api_url + '/api/projects');
    }
}
</script>

<template>
    <div class="row row-cols-5 g-4">

        <div class="col" v-for="project in projects">
            <div class="card project">
                <img :src="getImagePath(project.thumb)" alt="img not available">
                <div class="card-body">
                    <div class="card-title">
                        <strong>Titolo Progetto: </strong>
                        <span v-if="projects">{{ project.title }}</span>
                        <span v-else> no title </span>
                    </div>

                    <div class="card-title">
                        <strong>Linguaggio: </strong>
                        <span v-if="projects">{{ project.language }}</span>
                        <span v-else>no lanaguge</span>
                    </div>
                    <div class="type">
                        <strong>Type: </strong>
                        <span v-if="project.type">
                            {{ project.type.name }}
                        </span>
                        <span v-else>No type selected</span>
                    </div>
                    <div class="technologies">
                        <strong>Technologies: </strong>
                        <template v-if="project.technologies.lenght > 0">
                            <span v-for="technology in project.technologies">
                                {{ technology.name }}
                            </span>
                        </template>
                        <template v-else>
                            <span>No technologies.</span>
                        </template>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<style lang="scss" scoped>
.col {
    img {
        height: inherit;
        object-fit: cover;
        border-radius: 0.375rem;
    }

    .project {
        height: 352px;

        .card-body {
            display: none;

            img {
                border-radius: 0;
            }

            .card-title,
            .card-text {
                font-size: 14px;
            }

            .card-text {
                height: 170px;
                overflow: auto;
            }
        }

    }

    .project:hover .card-body {
        display: inline-block;
    }

    .project:hover>img {
        display: none;
    }

}
</style>