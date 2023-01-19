
<script>
import axios from 'axios';

export default {
    name: 'ProjectCard',
    data() {
        return {
            projects: [],
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

        },
        getImagePath(path) {
            console.log(path);
            if (path) {
                return this.base_api_url + '/storage/' + path
            }
            return '/img/noimg.jpg'
        },
        prevPage(url) {
            console.log(url)
            this.getProjects(url)
        },
        nextPage(url) {
            console.log(url)
            this.getProjects(url)
        }
    },
    mounted() {
        this.getProjects(this.base_api_url + '/api/projects');
    }
}
</script>

<template>
    <div class="row row-cols-5 g-4">

        <div class="col" v-for="project in projects.data">
            <div class="card project">
                <img :src="getImagePath(project.thumb)" alt="img not available">
                <div class="card-body">
                    <div class="card-title">
                        <strong>Titolo Progetto: </strong>
                        <span v-if="projects.title">{{ project.title }}</span>
                        <span v-else> no title </span>
                    </div>

                    <div class="card-title">
                        <strong>Linguaggio: </strong>
                        <span v-if="projects.language">{{ project.language }}</span>
                        <span v-else>no language</span>
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
                        <template v-if="project.technologies.length > 0">
                            <span v-for="technology in project.technologies">
                                # {{ technology.name }}
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
    <nav aria-label="Page navigation" class="d-flex justify-content-center pt-5">
        <ul class="pagination">
            <li class="page-item" v-if="projects.prev_page_url" @click="prevPage(projects.prev_page_url)">
                <a class="page-link" aria-label="Previous">
                    <span aria-hidden="true">&laquo;</span>
                </a>
            </li>
            <li class="page-item active" aria-current="page"><a class="page-link" href="#">{{
                projects.current_page
            }}</a>
            </li>

            <li class="page-item" v-if="projects.next_page_url" @click="nextPage(projects.next_page_url)">
                <a class="page-link" aria-label="Next">
                    <span aria-hidden="true">&raquo;</span>
                </a>
            </li>
        </ul>
    </nav>
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