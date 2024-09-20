<template>
    <div>
        <v-dialog v-model="props.isVisible" max-width="500px">
        <v-card>
        <v-card-title class="headline">{{ movieDetails.title }}</v-card-title>
        <v-card-text>
          <p>{{ movieDetails.overview }}</p>
        </v-card-text>
        <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="closeDetail">Fermer</v-btn>
        </v-card-actions>
        </v-card>
    </v-dialog>
    
    </div>
</template>
<script setup>
import { ref, watch, defineProps, defineEmits, onMounted } from 'vue';
import axios from 'axios';
import config from '../config.json';

let movieDetails = ref({});

const props = defineProps ({
    movieId: {
      type: Number,
      required: true
    },
    isVisible: {
      type: Boolean,
      required: true
    }
})

// Requête pour avoir les infos d'un film d'après son id
onMounted(() => {
  axios.get(config.url.movie_detail + props.movieId, {
    params: {
      api_key: config.api_key
    }
  })
  .then((response) => {
      console.log(response.data);
      movieDetails.value = response.data;
    })
    .catch((error) => {
      console.error('Erreur lors de la récupération des données:', error)
    })
})

const emit = defineEmits(['closePopUp']);

// Emet un événement au clic du bouton de fermeture
const closeDetail = () => {
  emit('closePopUp');
}

</script>