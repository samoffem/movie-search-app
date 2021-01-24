<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="@/assets/img.jpg" alt="" class="featured-img">
        <div class="description">
          <h3>Naruto</h3>
          <p>lorem ipsum blah bhah blah blah</p>
        </div>

      </router-link>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="What are you looking for" v-model="search">
      <input type="submit" value="search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster">
            <div class="type">{{movie.Type}}</div>
          </div>
          <div class="detail">
            <p class="year">{{movie.Year}}</p>
            <h3>{{movie.Title}}</h3>
          </div>

        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import {ref} from 'vue';
import env from '../env'

export default {
  name: 'Home',
  setup(){
    const search = ref("");
    const movies = ref([]);

    const SearchMovies = ()=>{
      if (search.value !== ""){
        fetch(`http://www.omdbapi.com/?apikey=${env.apiKey}&s=${search.value}`)
        .then(resp=> resp.json())
        .then(data=>{
          movies.value = data.Search
          search.value = ""
        })
      }
    }

    return{
      search,
      movies,
      SearchMovies
    }
  }
}
</script>

<style scoped>
  .home{
    max-width: 600px;
    margin: 0 auto;
  }
  .feature-card{
    position: relative;
  }
  .featured-img{
    display: block;
    width: 100%;
    height: 300px;
    object-fit: cover;
    position: relative;
    z-index: 0;
  }
  .description{
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.6);
    padding: 16px;
    z-index: 1;
  }
  .description> h3{
    color: #fff;
    margin-bottom: 16px;
  }
  .description> p{
    color: #fff;
  }

  .search-box{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;
  }

  .search-box input{
    display: block;
    appearance: none;
    border: none;
    outline: none;
    background: none;
  }
  .search-box input[type="text"]{
    width: 100%;
    color: #fff;
    background: #496583;
    font-size: 20px;
    padding: 10px 16px;
    border-radius: 8px;
    margin-bottom: 15px;
    transition: 0.4s;
  }
  .search-box input[type="text"]::placeholder{
    color: #f3f3f3;
  }
  .search-box input[type="text"]:focus{
    box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
  }
  .search-box input[type="submit"]{
    width: 100%;
    max-width: 300px;
    background-color: #42b883;
    padding: 16px;
    border-radius: 8px;
    color: #fff;
    font-size: 20px;
    text-transform: uppercase;
    transition: 0.4s;
  }
  .search-box input[type="submit"]:active{
    background-color: #3b8070;
  }

  .movies-list{
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;
  }
  .movie{
    max-width: 50%;
    flex: 1 1 50%;
    padding: 16px 8px;
  }
  .movie-link{
    display: flex;
    flex-direction: column;
    height: 100%;
  }
  .product-image{
    position: relative;
    display: block;
  }
  .product-image img{
    display: block;
    width: 100%;
    height: 275px;
    object-fit: cover;
  }
  .type{
    position: absolute;
    padding: 8px 16px;
    background-color: #42b883;
    color: #fff;
    bottom: 16px;
    left: 0px;
    text-transform: capitalize;
  }
  .detail{
    background-color: #496583;
    padding: 16px 8px;
    flex: 1 1 100%;
    border-radius: 0 0 8px 8px ;
  }
  .year{
    color: #aaa;
    font-size: 14px;
  }
  .detail h3{
    color: #fff;
    font-weight: 600;
    font-size: 18px;
  }
</style>
