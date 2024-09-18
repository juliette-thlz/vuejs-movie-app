<template>
    <div>
        
        <v-dialog v-model="isVisible" max-width="500px">
        <v-card>
        <v-card-title class="headline">Titre de la Pop-Up</v-card-title>
        <v-card-subtitle>
            {{ subtitle }}
        </v-card-subtitle>
        <v-card-text>
            <slot></slot> <!-- Contenu dynamique à insérer dans la pop-up -->
        </v-card-text>
        <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" @click="close">Fermer</v-btn>
        </v-card-actions>
        </v-card>
    </v-dialog>
    
    </div>
</template>
<script setup>
import { ref, defineProps, defineEmits, onMounted } from 'vue';
import axios from 'axios';
import config from '../../config.json';

let movieDetails = ref([])

const props = defineProps ({
    movieId: {
      type: String,
      required: true
    },
    modelValue: {
        type: Boolean,
        required: true
    }
})

onMounted(() => {
  axios.get(config.movie_detail + props.movieId, {
    params: {
      api_key: config.api_key
    }
  })
  .then((response) => {
      movieDetails.value = response.data;
    })
    .catch((error) => {
      console.error('Erreur lors de la récupération des données:', error)
    })
})

const emit = defineEmits(['close-detail']);

const closeDetail = () => {
    // console.log("envoi add");
    emit('close-detail');
}

</script>