<template>
    <div class="media_card ">
        <img class="poster" :src="getPosterPic(media)" alt="">
        <div class="hover_side">
          <div class="hover_wrapper d-flex flex-column flex-grow-1">

            <figure class="backdrop">

              <img class="backdrop_pic" :src="getBackdropPic(media)" alt="">
              <img class="flag" :src="getFlag(media)" alt="">
              <span class="gradient"></span>
              <div class="back_title">
                {{media.title}}
              </div>

            </figure>
            <div class="description d-flex flex-column justify-content-between flex-grow-1">
              <p class="overview ">
                {{media.overview}}
              </p>

              <p class="mt-auto mb-1">Votes</p>
              <ul class="starLIst d-flex gx-1">
                  <li v-for="(star,i) in stars" :key="i">
                      <i :class="['fa-solid fa-star', i + 1 <= getFillStars(media) ? 'fill' : '']"></i> 
                  </li>
              </ul>

              <div class="get_details">
                <i class="fa-solid fa-plus">
                  <p class="icon_cast">
                    Discover the Cast
                  </p>
                </i>
                <ul class="actors">
                  <li v-for="actor in cast" :key="actor.cast_id">
                    <img class="actor_pic" :src="getActorPic(actor.profile_path)" alt="">
                  </li>
                </ul>
              </div>
            </div>

          </div>

        </div>
    </div>
</template>

<script>
import axios from 'axios'

    // import axios from 'axios'  
    export default {
        name: 'MediaCard',

        props:{
            media: Object,
            stars: Array,
        },

        data(){
          return{
            enFlag: require('../assets/flags/en.jpg'),
            hoverSide: false,
            cast: []
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

            getCast(){
              const {id} = this.media
              const BASE_URI = 'https://api.themoviedb.org/3/movie/'

              axios
                  .get(`${BASE_URI}${id}/credits?api_key=867af6dea05b16c3445ca730fbe78a37`)
                  .then(res => {
                    
                    const cast = res.data.cast.slice(0, 5)
                    this.cast = cast

                    console.log(this.cast)
                  })
            },

            getActorPic(profile_pic){
              const BASE_URI = 'https://image.tmdb.org/t/p/w45/'
              const picUrl = BASE_URI + profile_pic

              return picUrl
            }

        },

        mounted(){
          this.getCast()
        }

    }
</script>

<style lang="scss" scoped>
  @import '../style/general.scss';
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

        .flag{
          position: absolute;
          top: 1rem;
          left: 1rem;
        }

        .backdrop_pic{
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
          overflow-y: scroll;
          margin-bottom: 0;
        }

        .get_details {
          display: flex;
          justify-content: center;
          align-items: center;
          color: $text-color;
          background-color: transparent;
          width: 30px;
          aspect-ratio: 1;
          border: solid $text-color 1px;
          border-radius: 50%;
          position: absolute;
          bottom: 1rem;
          right: 1rem;

          .icon_cast{
            display: none;
          }

          .actors{
            display: none;
          }
          
          &:hover {
            transition: all 300ms ease-in-out;
            justify-content: flex-start;
            background-color: $text-color;
            width: 200px;
            height: 30px;
            border-radius: 999px;
            padding: 0 8px;
            
            i{
              color: black;
              cursor: pointer;
              position: relative;

                &:hover .icon_cast{
                  font-size: 0.75rem;
                  padding: 10px;
                  background-color: rgba(245, 245, 245, 0.4);
                  display: block;
                  position: absolute;
                  transform: translateX(-50%);
                  top: -45px;
                  color: black;
                  width: fit;
                }
            }

            .actors{
              width: 100%;
              display: flex;
              justify-content: center;
              align-items: center;
              gap: 5px;
                img{
                  height: 28px;
                  aspect-ratio: 1;
                  object-fit: cover;
                  object-position: center;
                  border-radius: 50%;
                }
            }
          }
        }
        
      }
    }

  }
</style>