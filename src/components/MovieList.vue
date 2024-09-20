<template>
<v-container>
    <v-row v-if="movieFiltered && movieFiltered.length > 0">
      <!-- Boucle sur la liste des films -->
      <v-col v-for="movie in movieFiltered" :key="movie.id" cols="12" md="4" lg="3">
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
          <v-btn @click="showDetail(movie.id)">PLUS D'INFORMATIONS</v-btn>
        </v-card>
      </v-col>
    </v-row>
    <!-- Message de chargement si aucun film n'est récupéré -->
    <v-row v-else>
      <v-col cols="12" class="text-center">
        <p>Chargement...</p>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, watch, onMounted, defineProps, computed } from 'vue';
import axios from 'axios';
import config from '../config.json';

let movieList = ref([]);
let photo_path = config.url.photo_path;

// Props pour récupérer la valeur entrée dans la barre de recherche
const props = defineProps ({
    movieSearch: {
      type: String,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    }
})

// On récupère les listes avec une requête
const fetchMovies = () => {
  axios.get(config.url.movie_list + props.currentPage, {
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
};

// Appeler la fonction au lancement du composant
onMounted(() => {
  fetchMovies();
});

// Surveille les changements de page et fais la requête à chaque changement
watch(() => props.currentPage, () => {
  fetchMovies();
});

const emit = defineEmits(['movie-detail']);

// Fonction appelée sur le clic du bouton pour afficher les détails
// Elle émet un évement avec l'id du film
const showDetail = (movieId) => {
  console.log("show details", movieId);
  emit('movie-detail', movieId);
}


// Permet de retourner la liste de film si la recherche est vide, et seulement les films qui correspondent à la recherche
const movieFiltered = computed(() => {
  // Si la recherche est vide, retourne tous les films
  if (props.movieSearch === '') {
    return movieList.value;
  }
  // Sinon, retourne les films filtrés
  return movieList.value.filter(movie =>
    movie.title.toLowerCase().includes(String(props.movieSearch).toLowerCase())
  );
});

</script>