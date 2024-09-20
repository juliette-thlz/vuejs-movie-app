<template>
    <div>
        <v-container>
            <v-pagination
                v-model="currentPage"
                :length="totalPages"
            ></v-pagination>
        </v-container>
    </div>
</template>
<script setup>

import { ref, watch, defineEmits } from 'vue';

let currentPage = ref(1);
const totalPages = ref(10);

const emit = defineEmits(['paginationEmit']);

// Surveille les changements de page et émet un événement
watch(currentPage, (newPage) => {
    emit('paginationEmit', newPage);
});

</script>