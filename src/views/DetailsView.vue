<script setup>
import { ref, onMounted } from 'vue';
import { api } from '../apis/punk-api'
import { useRoute } from 'vue-router';

const route = useRoute();

const beer = ref();

onMounted(async () => {
  beer.value = await api.getBeerById(route.params.id);
});
</script>

<template>
  <div class="text-center">
    <div class="details p-3" v-if="beer">
      <h1 class="text-center pb-4">Details Page</h1>

      <img :src="beer.image_url" alt="picture of delicious beer" style="height: 200px">
      <h2>{{ beer.name }}</h2>
      <h4>Slogan: <i>{{ beer.tagline }}</i></h4>
      <p>{{ beer.description }}</p>
      <br />
      <p v-if="beer.brewers_tips"><b>Tip:</b> {{ beer.brewers_tips }}</p>
      <hr>

      <h3>Additional Information</h3>

      <p><b>Recommended Food Pairings:</b> {{ beer.food_pairing.join(", ") + "." }}</p>
      <p><b>IBU:</b> {{ beer.ibu }}</p>
      <p><b>pH:</b> {{ beer.ph }}</p>
      <p><b>Srm:</b> {{ beer.srm }}</p>
      <p><b>Abv:</b> {{ beer.abv }}</p>
      <p><b>Ebc:</b> {{ beer.ebc }}</p>
    </div>
    <div class="text-center" v-else>
      <!-- Loading delicious beer data... -->
    </div>
    <router-link class="btn btn-primary" to="/">Back</router-link>
  </div>
</template>

<style></style>
