<script>
import axios from 'axios';
export default {
    name: 'ContactsView',
    data() {
        return {
            api_base_url: 'http://127.0.0.1:8000',
            name: '',
            email: '',
            message: '',
            success: false,
            loading: false,
            errors: {}
        }
    },

    methods: {
        sendForm() {
            this.loading = true;
            this.errors = {};

            console.log(this.name);
            console.log(this.email);
            console.log(this.message);

            const data = {
                name: this.name,
                email: this.email,
                message: this.message
            }

            axios.post(`${this.api_base_url}/api/contacts`, data).then(response => {
                this.success = response.data.success;
                console.log(response);
                if (this.success) {
                    this.name = '';
                    this.email = '';
                    this.message = '';
                } else {
                    this.errors = response.data.success;
                }
                this.loading = false;
            });
        }
    }
}
</script>

<template>
    <div class="container">

        <p class="lead">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Libero, adipisci optio, tempore maiores sit
            asperiores
            sed est nulla esse quos quam laudantium, voluptatibus minus officiis assumenda. Ex vitae eos earum.
        </p>
        <div v-if="success" class="alert alert-success text-start" role="alert">
            Messaggio inviato con successo!
        </div>
        <div v-else class="alert alert-danger text-start" role="alert" v-for="(error) in errors">
            {{ error.name }}
        </div>
        <form @submit.prevent="sendForm()">
            <div class="mb-3">
                <label for="" class="form-label">Full Name</label>
                <input type="text" name="name" id="name" class="form-control" placeholder="Mario Rossi"
                    aria-describedby="fullNameHelper" v-model="name">
                <small id="fullNameHelper" class="text-muted">Add your full name</small>
            </div>
            <div class="mb-3">
                <label for="" class="form-label">Email</label>
                <input type="email" name="email" id="email" class="form-control" placeholder="mario.rossi@example.com"
                    aria-describedby="emailHelper" v-model="email">
                <small id="emailHelper" class="text-muted">Add your email address</small>
            </div>

            <div class="mb-3">
                <label for="" class="form-label">Message</label>
                <textarea class="form-control" name="message" id="message" rows="5" v-model="message"></textarea>
            </div>

            <button type="submit" class="btn btn-primary" :disable="loading">{{
                loading? 'Sending...': 'Contact Me'
            }}
            </button>
        </form>
    </div>
</template>


<style lang="scss" scoped>

</style>