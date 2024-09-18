<template>
    <div>
        <!-- list of movies -->
        <ul v-if="movieList.length">
            <li v-for="movie in movieList" :key="movie.id">
                <h3>{{ movie.original_title }}</h3>
            </li>
        </ul>
        <p v-else>Chargement...</p> <!-- Message en cas de chargement -->
    </div>
</template>
<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import config from '../../config.json';

let movieList = ref([]);

onMounted(() => {
  axios.get(' https://api.themoviedb.org/3/movie/popular', {
    params: {
      api_key: config.api_key
    }
  })
  .then((response) => {
      movieList.value = response.data.results;
    })
    .catch((error) => {
      console.error('Erreur lors de la récupération des données:', error)
    })
})

</script>