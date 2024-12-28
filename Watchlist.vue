<template>
  <div class="watchlist-container">
    <h2>My Watchlist</h2>
    
    
    <div v-if="watchlist.length === 0">
      <p>Your watchlist is empty.</p>
    </div>
    

    <div v-else class="movie-list">
      <div v-for="movie in watchlist" :key="movie.tmdb_id" class="movie-card">
        <img :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`" alt="Movie Poster" class="movie-poster" />
        <div class="movie-info">
          <h3>{{ movie.title }}</h3>
          <p>{{ movie.overview }}</p>
          <button @click="removeFromWatchlist(movie.tmdb_id)" class="delete-button">Remove</button>
        </div>
      </div>
    </div>
    
    
    <div v-if="error" class="error-message">
      {{ error }}
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      watchlist: [],  
      error: ''       
    };
  },
  async mounted() {
    await this.fetchWatchlist();
  },
 
  methods: {
  async fetchWatchlist() {
    const token = localStorage.getItem('token');
    if (!token) {
      this.error = 'You need to be logged in to view your watchlist.';
      return;
    }

    try {
      const response = await axios.get('http://localhost:8000/watchlist/', {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      });
      this.watchlist = response.data;
    } catch (error) {
      this.error = 'Error fetching your watchlist. Please try again later.';
    }
  },

  async removeFromWatchlist(movieId) {
    const token = localStorage.getItem('token');
    if (!token) {
      this.error = 'You need to be logged in to modify your watchlist.';
      return;
    }

    try {
      await axios.delete(`http://localhost:8000/watchlist/delete/${movieId}/`, {
        headers: {
          Authorization: `Bearer ${token}`,
        },
      });
      // Remove the movie from the watchlist array
      this.watchlist = this.watchlist.filter((movie) => movie.tmdb_id !== movieId);
    } catch (error) {
      this.error = 'Error removing the movie from your watchlist. Please try again later.';
    }
  },
},

};

</script>

<style scoped>
.watchlist-container {
  padding: 20px;
}

.movie-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.movie-card {
  width: 200px;
  border: 1px solid #ccc;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.movie-poster {
  width: 100%;
  height: auto;
}

.movie-info {
  padding: 10px;
}

.movie-info h3 {
  font-size: 1.2rem;
  margin: 10px 0;
}

.movie-info p {
  font-size: 1rem;
  color: #555;
}

.error-message {
  color: red;
  font-size: 1.2rem;
  text-align: center;
}

.delete-button {
  padding: 10px 20px;
  background-color: #4a90e2;
  border: none;
  color: white;
  cursor: pointer;
  font-size: 1rem;
  margin-top: 10px;
  border-radius: 5px;
}

.delete-button:hover {
  background-color: #357abd;

}

</style>
