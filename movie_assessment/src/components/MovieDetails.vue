<template>
  <div class="container mt-5">
    <div v-if="movie" class="card">
      <img :src="movie.Poster" class="card-img-top" alt="Movie Poster">
      <div class="card-body">
        <h2 class="card-title">{{ movie.Title }}</h2>
        <p class="card-text"><strong>Plot:</strong> {{ movie.Plot }}</p>
        <p class="card-text"><strong>Released:</strong> {{ movie.Released }}</p>
        <p class="card-text"><strong>Genre:</strong> {{ movie.Genre }}</p>
        <p class="card-text"><strong>Actors:</strong> {{ movie.Actors }}</p>
        <p class="card-text"><strong>Writers:</strong> {{ movie.Writer }}</p>
        <p class="card-text"><strong>Director:</strong> {{ movie.Director }}</p>
        <p class="card-text"><strong>Awards:</strong> {{ movie.Awards }}</p>
        <p class="card-text"><strong>Languages:</strong> {{ movie.Language }}</p>
        <p class="card-text"><strong>imdbRating:</strong> {{ movie.imdbRating }}</p>
      </div>
      <router-link :to="{ name: 'MovieList' }" class="btn btn-primary mt-3">Back to Movies List</router-link>
    </div>
    <div v-else>
      <p>No Data Found!</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['title'],
  data() {
    return {
      movie: null
    };
  },
  async created() {
    await this.getMovieDetails();
  },
  watch: {
    title: 'getMovieDetails'
  },
  methods: {
    async getMovieDetails() {
      try {
        const encodedTitle = encodeURIComponent(this.title).replace(/%20/g, '+');
        const response = await axios.get(`http://localhost:8080/api/movies/search?title=${encodedTitle}`);
        this.movie = response.data;
      } catch (error) {
        console.error(error);
        this.movie = null;
      }
    }
  }
}
</script>

<style scoped>
.container {
  max-width: 800px;
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
