<template>
  <div id="app">
    <div class="main_header container-fluid d-flex justify-content-between py-3">
      <div class="logo me-auto">
        BOOLFLIX
      </div>
      <input v-model="query" type="search" name="searchbar_movie" id="searchbar_1">
      <button @click="fetchList()" class="ms-1">search</button>
    </div>

    <main class="container">

      <ul class="movieList"> <b>Movies</b>
        <li v-for="movie in movieList" :key="movie.id">
          <img :src="getPosterPic(movie)" alt="">
            original title: {{ movie.original_title }} | title: {{movie.title}} <br>
            Lang: <img class="flag" :src="getFlag(movie)" alt="">  | Ratings: 
            <ul class="starLIst d-flex gx-1">
              <li v-for="(star,i) in stars" :key="i">
                <i :class="['fa-solid fa-star', i + 1 <= getFillStars(movie) ? 'fill' : '']"></i> 
              </li>
            </ul>

        </li>
      </ul>

      <ul class="tvList"> <b>Series</b>
        <li v-for="tv in tvList" :key="tv.id">
            <img :src="getPosterPic(tv)" alt="">
            original title: {{ tv.original_name }} | title: {{tv.name}} <br>
            Lang: <img class="flag" :src="getFlag(tv)" alt=""> | Ratings:
            <ul class="starLIst d-flex gx-1">
              <li v-for="(star,i) in stars" :key="i">
                <i :class="['fa-solid fa-star', i + 1 <= getFillStars(tv) ? 'fill' : '']"></i> 
              </li>
            </ul>
        </li>
      </ul>

    </main>
  </div>
</template>

<script>
import axios from 'axios';


export default {
  name: 'App',

  data(){
    return {
      query: '',
      API_KEY: '867af6dea05b16c3445ca730fbe78a37',
      BASE_MOVIE_URI: 'https://api.themoviedb.org/3/search/movie?',
      BASE_TV_URI: 'https://api.themoviedb.org/3/search/tv?',
      movieList: [],
      tvList: [],
      enFlag: require('./assets/flags/en.jpg'),
      stars: []
    }
  },

  computed: {

  },

  methods: {
    fetchList(){
      this.fetchMovie()
      this.fetchTv()

      this.getStars()
    },

    fetchMovie(){
      axios
          .get(this.BASE_MOVIE_URI, {

            params: {
              api_key: '867af6dea05b16c3445ca730fbe78a37',
              query: this.query
            }
          })
          .then(res => {
            this.movieList = res.data.results
            this.getStars()
          })
    },

    fetchTv(){
      axios
          .get(this.BASE_TV_URI, {
            params: {
              api_key: this.API_KEY,
              query: this.query
            }
          })
          .then(res => {
            this.tvList = res.data.results
          })
    },

    getPosterPic(movie){
      const BASE_URI = 'https://image.tmdb.org/t/p/w342/'
      const posterUrl = BASE_URI + movie.poster_path

      return posterUrl
    },

    getFlag(movie){

      if(movie.original_language == 'en'){
        const flagUrl = this.enFlag
        return flagUrl
      } 

      const BASE_URI = 'https://flagcdn.com/w20/'
      const flagUrl = BASE_URI + movie.original_language + '.jpg'
      return flagUrl
    },
    getFillStars(movie){
      const movieVote = parseInt(movie.vote_average / 2) 
      return movieVote
    },
    getStars(){
      this.stars.length = 5
    }
  },

}
</script>

<style lang="scss">
  #app{
    ul{
      list-style: none;
      padding: 0;
      margin: 0
    }

    .flag {
    width: 20px;
    border-radius: 50%;
    aspect-ratio: 1;
    object-fit: cover;
  }

    .fa-star{
      color: rgba(0, 0, 0, 0.4);
  
      &.fill{
        color: rgb(242, 224, 28);
      }
    }
  
  }
  
</style>
