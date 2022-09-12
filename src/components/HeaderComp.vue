<template>
    <header class="main_header container-fluid d-flex justify-content-between py-3">
        <div class="logo me-auto">
            BOOLFLIX
        </div>
        <input v-model="query" type="search" name="searchbar_movie" id="searchbar_1">
        <button @click="fetchLists()" class="ms-1">search</button>
    </header>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'HeaderComp',

        data(){
            return {
                query: '',
                API_KEY: '867af6dea05b16c3445ca730fbe78a37',
                BASE_MOVIE_URI: 'https://api.themoviedb.org/3/search/movie?',
                BASE_TV_URI: 'https://api.themoviedb.org/3/search/tv?',
            }
        },

        methods: {
            fetchMovie(){
            axios
                .get(this.BASE_MOVIE_URI, {
                    params: {
                    api_key: this.API_KEY,
                    query: this.query
                    }
                })
                .then(res => {
                    this.$emit('onResMovie', res.data.results)
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
                    this.$emit('onResTvs', res.data.results)
                })
            },
            fetchLists(){
                this.fetchMovie()
                this.fetchTv()
            },
        }
    }
</script>

<style lang="scss" scoped>
    @import '../style/general.scss';

    input, button{
        color: $bg-color;
    }

</style>