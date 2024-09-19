<template>
    <MovieSearch />
    <!-- <MoviePagination /> -->
    <MovieList @movie-detail="openMovieDetail" />
    <MovieDetail v-if="isPopupVisible" :movieId="selectedMovieId" :isVisible="isPopupVisible" @closePopUp="closeMovieDetail" />
</template>

<script setup>
import { ref } from 'vue';
import MovieDetail from '../components/MovieDetail.vue'
import MovieList from '../components/MovieList.vue'
import MoviePagination from '../components/MoviePagination.vue'
import MovieSearch from '../components/MovieSearch.vue'

const isPopupVisible = ref(false);
const selectedMovieId = ref(null);

const emit = defineEmits(['openMovieDetail']);

const openMovieDetail = (movieId) => {
    
    selectedMovieId.value = movieId;
    isPopupVisible.value = true;
    console.log("openMovieDetail called with movieId:", movieId, isPopupVisible.value);
    emit('openMovieDetail', false);
};

const closeMovieDetail = () => {

    isPopupVisible.value = false;
    console.log("closeMovieDetail called");
};

</script>
