<script setup>
import {RouterView } from 'vue-router'
// import HelloWorld from './components/HelloWorld.vue'
import Navbar from './components/Navbar.vue'

import { ref } from 'vue';

const searchQuery = ref('');

const handleSearch = (query) => {
  searchQuery.value = query;
};
</script>

<template>
  <Navbar @searchBeers="handleSearch">
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="40" height="40" />
  </Navbar>

  <RouterView :search-query="searchQuery"/>
</template>

<style scoped>

</style>
