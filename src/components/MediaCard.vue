<template>
    <div class="media_card ">
        <img class="poster" :src="getPosterPic(media)" alt="">
        <div class="hover_side">
          <div class="hover_wrapper d-flex flex-column flex-grow-1">

            <figure class="backdrop">

              <img :src="getBackdropPic(media)" alt="">
              <span class="gradient"></span>
              <div class="back_title">
                {{media.title}}
              </div>

            </figure>
            <div class="description d-flex flex-column justify-content-between flex-grow-1">
              <p class="overview ">
                {{media.overview}}
              </p>
              <img class="flag" :src="getFlag(media)" alt="">

              <p class="mt-auto mb-1">Votes</p>
              <ul class="starLIst d-flex gx-1">
                  <li v-for="(star,i) in stars" :key="i">
                      <i :class="['fa-solid fa-star', i + 1 <= getFillStars(media) ? 'fill' : '']"></i> 
                  </li>
              </ul>

            </div>

          </div>

        </div>
    </div>
</template>

<script>
    export default {
        name: 'MediaCard',

        props:{
            media: Object,
            stars: Array
        },

        data(){
          return{
            enFlag: require('../assets/flags/en.jpg'),
            hoverSide: false
          }
        },

        methods: {
              getPosterPic(movie){
              const BASE_URI = 'https://image.tmdb.org/t/p/w342/'
              const posterUrl = BASE_URI + movie.poster
              
              return posterUrl
            },
              getBackdropPic(movie){
              const BASE_URI = 'https://image.tmdb.org/t/p/w300/'
              const posterUrl = BASE_URI + movie.backdrop
              
              return posterUrl
            },
        
            getFlag(movie){
              const BASE_URI = 'https://flagcdn.com/w20/'
            
              if(movie.lang == 'en'){
                const flagUrl = this.enFlag
                return flagUrl
              } 
              if(movie.lang == 'ja'){
                const flagUrl = BASE_URI + 'jp.jpg'
                return flagUrl
              } 
          
              const flagUrl = BASE_URI + movie.lang + '.jpg'
              return flagUrl
            },
        
            getFillStars(movie){
              const movieVote = Math.ceil(movie.vote / 2) 
              return movieVote
            },


        }
    }
</script>

<style lang="scss" scoped>
  .media_card{
    width: 100%;
    position: relative;

    &:hover .hover_side{
      opacity: 1.4;
    }
    .hover_side{
      display: flex;
      flex-direction: column;
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0px;
      background-color: black;
      opacity: 0;

      .backdrop{
        display: flex;
        align-items: flex-end;
        position: relative;
        height: 260px;

        img{
          height: 100%;
          width: 100%;
          object-fit: cover
        };

        .gradient{
          position: absolute;
          height: 100%;
          width: 100%;
          z-index: 2;
          background: linear-gradient(0deg, rgba(0,0,0,1) 0%, rgba(219,219,219,0) 100%);
        }
        .back_title{
          font-size: 1.75rem;
          padding-left: 1rem;
          position: absolute;
          bottom: 6px;
          font-weight: 700;
          z-index: 3;
        }
      }

      .description {
        padding: 1rem;
        position: relative;

        .overview{
          max-height: 150px;
          overflow: scroll;
          margin-bottom: 0;
        }

        .flag{
          position: absolute;
          bottom: 1rem;
          right: 1rem;
        }
      }
    }

  }
</style>