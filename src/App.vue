<template>
  <div>
    <h1>Films Populaires</h1>
    <div v-if="loading">Chargement...</div>
    <div v-else>
      <div v-for="movie in movies" :key="movie.id">
        <h2>{{ movie.title }}</h2>
        <img :src="getImageUrl(movie.poster_path)" alt="Affiche du film" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const movies = ref([]);
const loading = ref(true);

const API_KEY = import.meta.env.VITE_TMDB_API_KEY;
const API_URL = `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}&language=fr-FR`;

onMounted(async () => {
  const res = await fetch(API_URL);
  const data = await res.json();
  movies.value = data.results;
  loading.value = false;
});

const getImageUrl = (path) => `https://image.tmdb.org/t/p/w200${path}`;
</script>
