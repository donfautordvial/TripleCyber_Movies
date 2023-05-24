<template>
    <div class="movie-detail">
      <router-link :to="'/'"  class="movie-router">  <h1 class="page-title">← Atrás...</h1></router-link>
      <h1 class="page-title">Detalle de la película</h1>
      <div class="movie-container">
        <img :src="getImageUrl(movie.poster_path)" alt="Poster" class="movie-poster">
        <div class="movie-details">
          <h2 class="movie-title">{{ movie.title }}</h2>
          <p class="movie-release-date">Release: {{ formatDate(movie.release_date) }}</p>
          <p class="movie-rating">Rating: {{ movie.vote_average }}/10</p>
          <br>
          <p class="movie-rating">{{ movie.overview }}</p>
          <!-- <button @click="addToFavorites(movie)" class="btn-add-favorite">Agregar a favoritos</button> -->
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        movie: {},
        favorites: []
      };
    },
    mounted() {
      const movieId = this.$route.params.id;
      axios.get(`https://api.themoviedb.org/3/movie/${movieId}`, {
        params: {
          api_key: 'bf091621962bdf5c30339e874a2a0c1a',
          language: 'en-US',
        },
      })
        .then(response => {
          this.movie = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
    methods: {
      addToFavorites(movie) {
        if (!this.isFavorite(movie)) {
            this.favorites.push(movie);
            this.saveFavorites();
        }
      },
      getImageUrl(path) {
        return `https://image.tmdb.org/t/p/w200${path}`;
      },
      formatDate(date) {
        const options = { year: 'numeric', month: 'long', day: 'numeric' };
        return new Date(date).toLocaleDateString('en-US', options);
      },
    },
  };
  </script>
  
  <!-- <style scoped> -->
  <style>
  .movie-detail {
    margin-top: 40px;
  }
  
  .page-title {
    font-size: 24px;
    margin-bottom: 20px;
    color: white;
  }
  
  .movie-container {
    /* display: flex; */
    align-items: center;
    align-self: center;
  }
  
  .movie-poster {
    width: 200px;
    height: 300px;
    object-fit: cover;
  }
  
  .movie-details {
    flex-grow: 1;
    height: 80px;
    /* margin: 20px; */
  }
  
  .movie-title {
    font-size: 18px;
    margin-bottom: 5px;
  }
  
  .movie-release-date,
  .movie-rating {
    margin: 0;
    font-size: 14px;
    color: #666;
  }
  
  .btn-add-favorite {
    padding: 8px 16px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-add-favorite:hover {
    background-color: #0056b3;
  }
  </style>
  