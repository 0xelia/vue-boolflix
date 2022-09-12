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
