<template>
    <div>
        <v-text-field placeholder="Rechercher un film" v-model="data"/>
        <!-- <p>Value: {{ data }}</p> -->
    </div>
</template>

<script setup>
import { ref, watch } from 'vue';

let data = ref('');

const emit = defineEmits(['searchMovieEmit'], data);

// Permet de surveiller l'input à chaque changement et émet un événement au parent
watch(data, (newValue) => {
    emit('searchMovieEmit', newValue);  
});


</script>