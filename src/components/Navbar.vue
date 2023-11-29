<script setup>
import { ref, watch } from 'vue';

const searchQuery = ref('');

const emit = defineEmits(['searchBeers']);

const submitSearch = () => {
    emit('searchBeers', searchQuery.value);
};

// Search on Keystroke
watch(searchQuery, () => {
    submitSearch()
});

</script>

<template>
    <div>
        <nav class="navbar navbar-expand-lg bg-body-tertiary shadow-sm">
            <div class="container-fluid">
                <div class="d-flex justify-content-center align-items-center">
                    <img alt="Vue logo" class="logo me-1" src="@/assets/logo.svg" width="30" height="30" />
                    <a class="navbar-brand" href="#">Brewhaus</a>
                </div>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                    data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
                    aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarSupportedContent">
                    <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                        <li class="nav-item">
                            <router-link class="nav-link" aria-current="page" to="/">Home</router-link>
                        </li>
                    </ul>
                    <form class="d-flex" role="search" @submit.prevent="submitSearch">
                        <input class="form-control me-2" type="search" placeholder="Search by Title!" aria-label="Search" v-model="searchQuery">
                        <!-- <button class="btn btn-outline-success" type="submit">Search</button> -->
                    </form>
                </div>
            </div>
        </nav>
    </div>
</template>

<style>

</style>

