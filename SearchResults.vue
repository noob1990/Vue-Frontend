<template>
  <div class="search-results">
    <h2>Search Results for "{{ $route.query.query }}"</h2>
    <div v-if="loading" class="loading">Loading...</div>
    <div v-if="error" class="error">{{ error }}</div>
    <div v-if="results.length === 0 && !loading" class="no-results">No results found.</div>
    <div class="movie-list" v-if="results.length > 0">
      <div v-for="movie in results" :key="movie.id" class="movie-card">
        <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="Movie Poster" />
        <h3>{{ movie.title }}</h3>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      results: [],
      loading: true,
      error: '',
    };
  },
  async created() {
    const query = this.$route.query.query; 
    try {
      const apiKey = '6ae5bd9b271bceee451ec5fc107a29da';
      const url = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${query}`;
      const response = await axios.get(url);
      this.results = response.data.results;
    } catch (error) {
      this.error = 'Error fetching search results. Please try again later.';
    } finally {
      this.loading = false;
    }
  },
};
</script>

<style scoped>
.search-results {
  padding: 20px;
}
.movie-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}
.movie-card {
  width: 200px;
  text-align: center;
}
.movie-card img {
  width: 100%;
  border-radius: 8px;
}
</style>
