<template>
    <MovieSearch @searchMovieEmit="setMovieSearch"/>
    <MovieList @movie-detail="openMovieDetail" :movieSearch="movieSearchData" :currentPage="pageNumber" />
    <MovieDetail v-if="isPopupVisible" :movieId="selectedMovieId" :isVisible="isPopupVisible" @closePopUp="closeMovieDetail" />
    <MoviePagination @paginationEmit="changePage" />
</template>

<script setup>
import { ref } from 'vue';
import MovieDetail from '../components/MovieDetail.vue'
import MovieList from '../components/MovieList.vue'
import MoviePagination from '../components/MoviePagination.vue'
import MovieSearch from '../components/MovieSearch.vue'

const isPopupVisible = ref(false);
const selectedMovieId = ref(null);
const movieSearchData = ref('');
const pageNumber = ref(1);

// Met les valeurs récupérées par l'événement de movieList pour les mettre dans les props de movieDetail
const openMovieDetail = (movieId) => {
    selectedMovieId.value = movieId;
    isPopupVisible.value = true;
    console.log("openMovieDetail called with movieId:", movieId, isPopupVisible.value);
};

// Au déclenchement de l'événement pour fermer la pop-up dans movieDetail, on cache la pop-up
const closeMovieDetail = () => {
    isPopupVisible.value = false;
    console.log("closeMovieDetail called");
};

// Permet de passer la valeur de recherche (dans movieSearch) à la props de movieList
const setMovieSearch = (data) => {
    movieSearchData.value = data;
    console.log("setMovieSearch", movieSearchData.value);
}

// Permet de passer le numéro de page (dans moviePagination) à la props de movieList
const changePage = (page) => {
    console.log("changePage", page);
    pageNumber.value = page;
}

</script>
