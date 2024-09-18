<template>
<v-container>
    <v-row>
      <!-- Boucle sur la liste des films -->
      <v-col v-for="movie in movieList" :key="movie.id" cols="12" md="4" lg="3">
        <v-card>
          <v-img
            :src="photo_path + movie.poster_path"
            alt="Poster du film"
            aspect-ratio="1.7"
          ></v-img>

          <v-card-title>{{ movie.title }}</v-card-title>

          <v-card-subtitle>{{ movie.release_date }}</v-card-subtitle>

          <v-card-text>
            {{ movie.overview.length > 100 ? movie.overview.substring(0, 100) + '...' : movie.overview }}
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Message de chargement si aucun film n'est récupéré -->
    <v-row v-if="movieList.length === 0">
      <v-col cols="12" class="text-center">
        <p>Chargement...</p>
      </v-col>
    </v-row>
  </v-container>
</template>
<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import config from '../../config.json';

let movieList = ref([]);
let photo_path = config.photo_path;

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