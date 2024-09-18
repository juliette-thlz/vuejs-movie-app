<template>
    <MovieSearch />
    <MovieList />
    <!-- <MoviePagination /> -->
    <MovieList @showMovieDetailEmit="openMovieDetail"/>
    <MovieDetail :movieId="selectedMovieId" v-model:modelValue="isPopupVisible" />
</template>

<script setup>
import { ref } from 'vue';
import MovieDetail from '../components/MovieDetail.vue'
import MovieList from '../components/MovieList.vue'
import MoviePagination from '../components/MoviePagination.vue'
import MovieSearch from '../components/MovieSearch.vue'

const isPopupVisible = ref(false);
// const selectedMovieId = ref(null);
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

const emit = defineEmits(['showPopUp']);

const openMovieDetail = (movieId) => {
    console.log("openMovieDetail", movieId);
    selectedMovieId.value = movieId;
    isPopupVisible.value = true;
    emit('showPopUp');
};

</script>
