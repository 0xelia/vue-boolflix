<template>
    <div>
        <img :src="getPosterPic(tv)" alt="">
            original title: {{ tv.original_name }} | title: {{tv.name}} <br>
            Lang: <img class="flag" :src="getFlag(tv)" alt=""> | Ratings:
            <ul class="starLIst d-flex gx-1">
                <li v-for="(star,i) in stars" :key="i">
                    <i :class="['fa-solid fa-star', i + 1 <= getFillStars(tv) ? 'fill' : '']"></i> 
                </li>
            </ul>
    </div>
</template>

<script>
    export default {
        name: 'CardTv',
        props: {
            tv: {
                type: Object,
                required: true
            },
            stars: Array
        },

        data(){
            return{
                enFlag: require('../assets/flags/en.jpg'),
            }
        },

        methods: {
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
    }
    }
</script>

<style lang="scss" scoped>

</style>