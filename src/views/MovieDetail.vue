<template>
    <div class="movie-detail">
        <h2>{{movie.Title}}</h2>
        <p>{{movie.Year}}</p>
        <img :src="movie.Poster" alt="Movie Poster" class="poster-img">
        <p>{{movie.Plot}}</p>
        
    </div>
</template>

<script>
import {ref, onBeforeMount} from 'vue'
import {useRoute} from 'vue-router'
import env from '../env'

export default {
    setup(){
        const movie = ref({})
        const route = useRoute()

        onBeforeMount(()=>{
            fetch(`http://www.omdbapi.com/?apikey=${env.apiKey}&i=${route.params.id}&plot=full`)
            .then(response =>response.json())
            .then(data=>{
                movie.value = data
                console.log(movie.value)
            })
        })

        return{
            movie
        }
    }
}
</script>

<style>
    .movie-detail{
        padding: 16px;
    }
    .movie-detail h2{
        color: #fff;
        font-weight: 600;
        font-size: 28px;
        margin-bottom: 16px;
    }
    .poster-img{
        display: block;
        max-width: 200px;
        margin-bottom: 16px;
    }
    .movie-detail p{
        color: #ffffff;
        font-size: 18px;
        line-height: 1.4;
    }
</style>