
<script>

export default {
    name: 'ProjectCard',
    props: {
        project: Object
    },
    data() {
        return {
            base_api_url: 'http://127.0.0.1:8000',
        }
    },
    methods: {

        getImagePath(path) {
            console.log(path);
            if (path) {
                return this.base_api_url + '/storage/' + path
            }
            return '/img/noimg.jpg'
        },
    },
}
</script>

<template>


    <div class="col">
        <div class="card project">
            <img :src="getImagePath(project.thumb)" alt="img not available">
            <div class="card-body">
                <div class="card-title">
                    <strong>Titolo Progetto: </strong>
                    <span v-if="project.title">{{ project.title }}</span>
                    <span v-else> no title </span>
                </div>

                <div class="card-title">
                    <strong>Linguaggio: </strong>
                    <span v-if="project.language">{{ project.language }}</span>
                    <span v-else>no language</span>
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
                <router-link :to="{ name: 'single-project', params: { slug: project.slug } }">Read
                    More</router-link>
            </div>
        </div>
    </div>


</template>

<style lang="scss" scoped>
.col {
    img {
        height: inherit;
        object-fit: scale-down;
        //border-radius: 0.375rem;
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