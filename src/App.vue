<template>
  <div id="app">
    <HeaderComp @onResMovie="getMovies" @onResTvs="getTvs"/>

    <main class="container">
      <h3 class="title">Movies</h3>
      <ul class="movieList row row-cols-lg-3 row-cols-md-2 row-cols-1 justify-content-lg-start justify-content-center"> 
        <li v-for="movie in movies" :key="movie.id" class=" media_wrapper d-flex justify-content-center justify-content-lg-start col">

          <MediaCard :stars="stars" :media="movie"/>

        </li>
      </ul>

      <h3 class="title">Series</h3>
      <ul class="tvList row row-cols-lg-3 row-cols-md-2 row-cols-1 justify-content-lg-start justify-content-center"> 
        <li v-for="tv in tvs" :key="tv.id" class=" media_wrapper d-flex justify-content-center justify-content-lg-start col">
            
          <MediaCard :stars="stars" :media="tv"/>

        </li>
      </ul>

    </main>
  </div>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue';
import MediaCard from './components/MediaCard.vue';

export default {
  name: 'App',

  components: {
    HeaderComp,
    MediaCard
},

  data(){
    return {
      movieList: [],
      tvList: [],
      stars: []
    }
  },

  computed: {
    movies(){
      return this.movieList.map(movie => {
        const movies = {
          id: movie.id,
          title: movie.title,
          original_title: movie.original_title,
          lang: movie.original_language,
          vote: movie.vote_average,
          poster: movie.poster_path,
          backdrop: movie.backdrop_path,
          overview: movie.overview
        }
        return movies
      })
    },

    tvs(){
      return this.tvList.map(movie => {
        const movies = {
          id: movie.id,
          title: movie.name,
          original_title: movie.original_name,
          lang: movie.original_language,
          vote: movie.vote_average,
          poster: movie.poster_path,
          backdrop: movie.backdrop_path,
          overview: movie.overview
        }
        return movies
      })
    }


  },

  methods: {
    getMovies(data){
      this.movieList = data
    },
    getTvs(data){
      this.tvList = data
    },

    getStars(){
      this.stars.length = 5
    }

  },

  mounted(){
    this.getStars()
  }

}
</script>

<style lang="scss">
  @import './style/general.scss';
  #app{
    background-color: $bg-color;
    height: 100vh;
    overflow: auto;

    ul, li{
      padding: 0;
      margin: 0;
    }

    img{
      width: 100%;
    }

    .title{
      padding: 1.5rem 0 3rem 0;
      font-size: 2rem;
    }
    .media_wrapper{
      padding: 0.5rem 0.5rem;

    }

    .flag {
    width: 20px;
    border-radius: 50%;
    aspect-ratio: 1;
    object-fit: cover;
  }

    .fa-star{
      color: rgba(199, 199, 199, 0.4);
  
      &.fill{
        color: rgb(242, 224, 28);
      }
    }
  
  }
  
</style>
