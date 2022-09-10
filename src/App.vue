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
            original title: {{ movie.original_title }} | title: {{movie.title}} <br>
            Lang: {{movie.original_language}} | Ratings: {{movie.vote_average}}
        </li>
      </ul>
      <ul class="movieList"> <b>Series</b>
        <li v-for="tv in tvList" :key="tv.id">
            original title: {{ tv.original_name }} | title: {{tv.name}} <br>
            Lang: {{tv.original_language}} | Ratings: {{tv.vote_average}}
        </li>
      </ul>
    </main>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',

  data(){
    return {
      query: '',
      API_KEY: '867af6dea05b16c3445ca730fbe78a37',
      BASE_MOVIE_URI: 'https://api.themoviedb.org/3/search/movie?',
      BASE_TV_URI: 'https://api.themoviedb.org/3/search/tv?',
      movieList: [],
      tvList: []
    }
  },

  methods: {
    fetchList(){
      this.fetchMovie()
      this.fetchTv()
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

            console.log(this.movieList)
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

            console.log(this.tvList)
          })
    },
  },

  
}
</script>

<style lang="scss">

</style>
