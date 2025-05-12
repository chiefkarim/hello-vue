<template>
  <div class="min-h-screen min-w-[70vw] bg-gray-100 p-6">
    <h1 class="text-3xl font-bold mb-6 text-center text-gray-800">
      Popular Movies
    </h1>
    <button @click="filterHighRated" class="p-2 bg-white m-2 outline-1">
      filter high rated movies
    </button>

    <button @click="getmovies" class="p-2 bg-white m-2 outline-1">reset</button>
    <div v-if="state.loading" class="text-center text-gray-600">
      Chargement...
    </div>

    <div
      v-else
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <div
        v-for="movie in state.movies"
        :key="movie.id"
        class="bg-white shadow-md rounded-lg overflow-hidden hover:shadow-lg transition-shadow"
      >
        <img
          :src="getImageUrl(movie.poster_path)"
          alt="Affiche du film"
          class="w-full h-72 object-cover"
        />
        <div class="p-4">
          <h2 class="text-lg font-semibold text-gray-800">
            {{ movie.title }}
            <span>{{ movie.vote_average }}</span>
          </h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, reactive, onMounted } from "vue";

const state = reactive({ loading: true, movies: [] });
// TODO : à sécuriser plus tard (via proxy ou Nuxt runtime config)
const API_KEY = import.meta.env.VITE_TMDB_API_KEY;
const API_URL = `https://api.themoviedb.org/3/movie/top_rated?api_key=${API_KEY}&language=en-EN`;
async function getmovies() {
  const res = await fetch(API_URL);
  const data = await res.json();
  state.movies = data.results;
  state.loading = false;
}
onMounted(getmovies());

const highRatedMovies = computed(() => {
  return state.movies?.filter((movie) => movie.vote_average > 8.6);
});

function filterHighRated() {
  state.movies = highRatedMovies.value;
}
const getImageUrl = (path) => `https://image.tmdb.org/t/p/w500${path}`;
</script>
