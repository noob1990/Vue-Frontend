<template>
  <header class="header">
    <nav class="nav">
      <div class="logo">
        <h1>BingePlay</h1>
      </div>

      
      <ul class="nav-links">
        <router-link to="/" class="nav-button">Movies</router-link>
        <router-link to="/watchlist" class="nav-button">My List</router-link>
      </ul>



      
      <div class="auth-links">

        <div class="search-bar">
          <input 
            type="text" 
            v-model="searchQuery" 
            @keyup.enter="performSearch" 
            placeholder="Search movies..." 
            class="search-input" />
          <button @click="performSearch" class="search-button">Search</button>
        </div>

        <router-link v-if="!isAuthenticated" to="/signup" class="auth-button">Sign Up</router-link>
        <router-link v-if="!isAuthenticated" to="/signin" class="auth-button">Sign In</router-link>
        <button v-if="isAuthenticated" @click="signOut" class="auth-button">Sign Out</button>
      </div>

    </nav>
  </header>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      isAuthenticated: false, 
      searchQuery: '',
    };
  },
  mounted() {
    this.checkAuthentication();
  },
  methods: {
    checkAuthentication() {
      const token = localStorage.getItem('token');
      if (token) {
        this.isAuthenticated = true;
      }
    },
    signOut() {
      localStorage.removeItem('token');
      this.isAuthenticated = false;
      this.$router.push('/');
    },
    performSearch() {
      if (this.searchQuery.trim()) {
        this.$router.push({ name: 'SearchResults', query: { query: this.searchQuery.trim() } });
      }
    }
    
  },
  watch: {
    // Watch for route changes and recheck authentication status
    '$route'(to, from) {
      this.checkAuthentication();
    }
  }
};
</script>




<style scoped>
/* Styles for the header */
.header {
  background-color: #111;
  padding: 10px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.nav {
  display: flex;
  justify-content: space-between;
  width: 100%;
  align-items: center;
}

.logo h1 {
  color: #fff;
  font-size: 2rem;
  margin: 0;
}

.nav-links {
  list-style-type: none;
  display: flex;
  gap: 20px;
  margin: 0;
}

.nav-button {
  color: #fff;
  font-size: 1.1rem;
  cursor: pointer;
  transition: color 0.3s;
}

.nav-button:hover {
  color: #e74c3c;
}

.auth-links {
  display: flex;
  gap: 10px;
}

.auth-button {
  color: #fff;
  padding: 8px 16px;
  background-color: #4a90e2;
  border-radius: 4px;
  font-size: 1.1rem;
  transition: background-color 0.3s;
  cursor: pointer;
}

.auth-button:hover {
  background-color: #357abd;
}

.auth-button:active {
  background-color: #357abd;
}

.search-button {
  background-color: black; /* Button background color */
  color: white;           /* Text color */
  border: none;           /* Removes border */
  border-radius: 4px;     /* Rounded corners */
  padding: 5px 10px;      /* Small padding for a compact button */
  font-size: 14px;        /* Slightly smaller font size */
  cursor: pointer;        /* Pointer cursor on hover */
  transition: background-color 0.3s ease; /* Smooth transition for hover effect */
}

.search-button:hover {
  background-color: #333; /* Darker black on hover */
}

.search-button:active {
  background-color: #555; /* Slightly lighter black on click */
}
</style>
