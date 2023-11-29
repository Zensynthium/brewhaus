<script setup>
import { ref, onMounted, computed, watch } from 'vue';
import { api } from '../apis/punk-api'

const props = defineProps({
  searchQuery: String
});

const page = ref(1);

const previousPage = () => {
  if (page.value == 1) {
    return
  }

  return --page.value
};

const nextPage = () => {
  ++page.value
};

const setPage = (pageNum) => {
  // NOTE: Max number is 11 if we're using 36 per page layout
  if (pageNum > 0 && pageNum < 11) {
    
    page.value = pageNum
  }
};

watch(page, async (newPageNum) => {
  beers.value = await api.getBeers({ page: newPageNum, perPage: 36 })
});


const beers = ref([]);
const filteredBeers = computed(() => {
  if (!props.searchQuery) {
    return beers.value
  }

  const filteredBeers = beers.value.filter(beer =>
    beer.name.toLowerCase().includes(props.searchQuery.toLowerCase())
  );

  return filteredBeers
});

const truncateText = (text) => {
  const maxLength = 100;

  return text.length > maxLength
    ? text.substring(0, maxLength) + '...'
    : text;
};

onMounted(async () => {
  beers.value = await api.getBeers();
});
</script>

<template>
  <main>
    <!-- Three in a row on large, two in a row medium, and one in a row on mobile-->
    <h1 class="text-center my-4">Brewhaus Beers 🍻</h1>
    <div v-if="beers" class="row flex-row flex-wrap justify-content-evenly w-100">
      <div class="mb-3 col-12 col-lg-4" v-for="beer in filteredBeers" :key="beer.id">
        <div class="card m-2 p-2 shadow border-0">
          <div class="row g-0">
            <div class="col-md-4 d-flex justify-content-center align-items-center">
              <img :src="beer.image_url" class="img-fluid rounded-start m-auto" style="max-height: 250px"
                alt="picture of delicious beer">
            </div>
            <div class="col-md-8">
              <div class="card-body text-center">
                <h5 class="card-title">{{ beer.name }}</h5>
                <p class="card-text">{{ truncateText(beer.description) }}</p>
                <router-link :to="`/details/${beer.id}`" class="btn btn-primary m-auto">Details</router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="row flex-row flex-wrap justify-content-evenly w-100">
      <div class="mb-3 col-12 col-lg-4" v-for="index in 36" :key="index">
        <div class="card m-2 p-2 shadow border-0">
          <div class="row g-0">
            <div class="col-md-4 d-flex justify-content-center align-items-center">
              <img src="" class="img-fluid rounded-start m-auto" style="max-height: 250px"
                alt="picture of delicious beer">
            </div>
            <div class="col-md-8">
              <div class="card-body text-center">
                <h5 class="card-title placeholder-glow">
                  <span class="placeholder col-6"></span>
                </h5>
                <p class="card-text placeholder-glow">
                  <span class="placeholder col-7"></span>
                  <span class="placeholder col-4"></span>
                  <span class="placeholder col-4"></span>
                  <span class="placeholder col-6"></span>
                  <span class="placeholder col-8"></span>
                </p>
                <a class="btn btn-primary disabled placeholder col-6" aria-disabled="true"></a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <nav aria-label="Page Navigation" class="d-flex justify-content-center">
      <ul class="pagination bg-white">
        <li class="page-item"><a class="page-link" @click="previousPage()" href="#!">Previous</a></li>
        <li class="page-item" v-for="index in 6" :key="index"><a class="page-link" @click="setPage(index)" href="#!">{{ index }}</a></li>
        <li class="page-item"><a class="page-link" @click="nextPage()" href="#!">Next</a></li>
      </ul>
    </nav>
  </main>
</template>
