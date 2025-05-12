<template>
  <div class="min-h-screen bg-gray-100 p-6">
    <h1 class="text-3xl font-bold mb-6 text-center text-gray-800">
      Films Populaires
    </h1>

    <div v-if="loading" class="text-center text-gray-600">Chargement...</div>

    <div
      v-else
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <div
        v-for="movie in movies"
        :key="movie.id"
        class="bg-white shadow-md rounded-lg overflow-hidden hover:shadow-lg transition-shadow"
      >
        <img
          :src="getImageUrl(movie.poster_path)"
          alt="Affiche du film"
          class="w-full h-72 object-cover"
        />
        <div class="p-4">
          <h2 class="text-lg font-semibold text-gray-800">{{ movie.title }}</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const movies = ref([]);
const loading = ref(true);

// TODO : à sécuriser plus tard (via proxy ou Nuxt runtime config)
const API_KEY = import.meta.env.VITE_TMDB_API_KEY;
const API_URL = `https://api.themoviedb.org/3/movie/popular?api_key=${API_KEY}&language=fr-FR`;

onMounted(async () => {
  const res = await fetch(API_URL);
  const data = await res.json();
  movies.value = data.results;
  loading.value = false;
});

const getImageUrl = (path) => `https://image.tmdb.org/t/p/w500${path}`;
</script>
