<template>
  <div class="container mt-5">
    <search-bar @search="searchMovies" @clear="clearSearch"></search-bar>
    <div v-if="movies.length === 0" class="mt-3">
      <p>No movies found.</p>
    </div>
    <div v-else class="row mt-3">
      <div v-for="movie in movies" :key="movie.Title" class="col-md-4 mb-3">
        <div class="card h-100">
          <img :src="movie.Poster" class="card-img-top" alt="Movie Poster">
          <div class="card-body">
            <h5 class="card-title">{{ movie.Title }}</h5>
            <p class="card-text">{{ movie.Plot }}</p>
            <p class="card-text"><small class="text-muted">{{ movie.Year }}</small></p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './SearchBar.vue';

export default {
  components: {
    SearchBar
  },
  data() {
    return {
      movies: [],
      searchQuery: ''
    }
  },
  mounted() {
    this.getPopularMovies();
  },
  methods: {
    getPopularMovies() {
      axios.get(`http://localhost:8080/api/movies/popular`)
          .then(response => {
            this.movies = response.data;
          })
          .catch(error => {
            console.error(error);
          });
    },
    searchMovies(query) {
      this.searchQuery = query;
      if (query) {
        axios.get(`http://localhost:8080/api/movies/search?title=${query}`)
            .then(response => {
              this.movies = response.data ? [response.data] : [];
            })
            .catch(error => {
              console.error(error);
            });
      } else {
        this.getPopularMovies();
      }
    },
    clearSearch() {
      this.searchQuery = '';
      this.getPopularMovies();
    }
  }
}
</script>

<style scoped>
.container {
  width: 1200px;
}

.card-img-top {
  height: 400px;
  object-fit: cover;
}

.card-body {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>
