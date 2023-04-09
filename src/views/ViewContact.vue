<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">View Contact</p>
                <p class="fst-italic">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quis corporis incidunt
                    praesentium placeat nisi dolores provident repellat. Aspernatur in ut est voluptates quasi commodi sit
                    itaque voluptatum repellat. Enim consectetur doloribus quibusdam cum expedita accusantium, iusto ad ex,
                    cupiditate esse non qui accusamus odit, doloremque delectus eos quia saepe! Laboriosam?</p>
            </div>
        </div>
    </div>


    <!-- Spinner -->
    <div v-if="loading">
        <div class="container mt-3">
            <div class="row">
                <div class="col">
                    <LoadSpinner />
                </div>
            </div>
        </div>
    </div>

    <!-- Error Message -->
    <div v-if="!loading && errorMessage">
        <div class="container">
            <div class="row">
                <div class="col">
                    <p class="h3 text-danger fw-bold">{{ errorMessage }}</p>
                </div>
            </div>
        </div>
    </div>


    <!-- Contact View -->
    <div class="container" v-if="!loading && isDone()">
        <div class="row">
            <div class="col-md-4">
                <img :src="contact.photo" alt="" class="contact-img-big">
            </div>
            <div class="col-md-6">
                <ul class="list-group">
                    <li class="list-group-item">Name: <span>{{ contact.name }}</span></li>
                    <li class="list-group-item">Email: <span>{{ contact.email }}</span></li>
                    <li class="list-group-item">Mobile: <span>{{ contact.mobile }}</span></li>
                    <li class="list-group-item">Company: <span>{{ contact.company }}</span></li>
                    <li class="list-group-item">Title: <span>{{ contact.title }}</span></li>
                    <li class="list-group-item">Group: <span>{{ group.name }}</span></li>
                </ul>
            </div>
        </div>
        <div class="row mt-3">
            <div class="col">
                <router-link to="/" class="btn btn-success"><i class="fa fa-arrow-alt-circle-left"></i>Back</router-link>
            </div>
        </div>
    </div>
</template>
  
<script>
import { ContactService } from '@/services/ContactService';
import LoadSpinner from '@/components/LoadSpinner.vue';

export default {
    name: 'ViewContact',
    components: { LoadSpinner },
    data: function () {
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            contact: {},
            errorMessage: null
        }
    },

    created: async function () {
        try {
            this.loading = true;
            let response = await ContactService.getContact(this.contactId);
            let groupResponse = await ContactService.getGroup(response.data);
            this.contact = response.data;
            this.group = groupResponse.data;
            this.loading = false;
        } catch (error) {
            this.errorMessage = error,
                this.loading = false;
        }
    },

    methods: {
        isDone: function () {
            return Object.keys(this.contact).length > 0 && Object.keys(this.group).length > 0;
        }
    }
}
</script>
  
<style></style>
  