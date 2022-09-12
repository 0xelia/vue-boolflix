<template>
  <div id="app">
    <HeaderComp @onResMovie="getMovies" @onResTvs="getTvs"/>

    <main class="container">

      <ul class="movieList"> <b>Movies</b>
        <li v-for="movie in movieList" :key="movie.id">
          <CardMovie :movie="movie" :stars="stars"/>
        </li>
      </ul>

      <ul class="tvList"> <b>Series</b>
        <li v-for="tv in tvList" :key="tv.id">

          <CardTv :tv="tv" :stars="stars"/>

        </li>
      </ul>

    </main>
  </div>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue';
import CardMovie from './components/CardMovie.vue';
import CardTv from './components/CardTv.vue';

export default {
  name: 'App',

  components: {
    HeaderComp,
    CardMovie,
    CardTv
},

  data(){
    return {
      enFlag: require('./assets/flags/en.jpg'),
      movieList: [],
      tvList: [],
      stars: []
    }
  },

  computed: {

  },

  methods: {
    getMovies(movies){
      this.movieList = movies
    },
    getTvs(tvs){
      this.tvList = tvs
    },
    
    getPosterPic(movie){
      const BASE_URI = 'https://image.tmdb.org/t/p/w342/'
      const posterUrl = BASE_URI + movie.poster_path

      return posterUrl
    },

    getFlag(movie){
      const BASE_URI = 'https://flagcdn.com/w20/'

      if(movie.original_language == 'en'){
        const flagUrl = this.enFlag
        return flagUrl
      } 
      if(movie.original_language == 'ja'){
        const flagUrl = BASE_URI + 'jp.jpg'
        return flagUrl
      } 

      const flagUrl = BASE_URI + movie.original_language + '.jpg'
      return flagUrl
    },

    getFillStars(movie){
      const movieVote = Math.round(movie.vote_average / 2) 
      return movieVote
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

    .flag {
    width: 20px;
    border-radius: 50%;
    aspect-ratio: 1;
    object-fit: cover;
  }

    .fa-star{
      color: rgba(212, 212, 212, 0.4);
  
      &.fill{
        color: rgb(242, 224, 28);
      }
    }
  
  }
  
</style>
