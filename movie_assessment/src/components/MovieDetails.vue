<template>
  <div class="container mt-5">
    <div v-if="movie" class="card">
      <img :src="movie.Poster" class="card-img-top" alt="Movie Poster">
      <div class="card-body">
        <h2 class="card-title">{{ movie.Title }}</h2>
        <p class="card-text"><strong>Plot:</strong> {{ movie.Plot }}</p>
        <p class="card-text"><strong>Released:</strong> {{ movie.Released }}</p>
      </div>
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
