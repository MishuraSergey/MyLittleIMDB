<template>
  <div class="search_wrap">
    <div class="input_wrap">
      <input class="input_field" type="text" placeholder="" v-model="userInput" @keyup="titleSearch">
      <button class="input_btn" v-for="movie in movies" @click="getMovieDetails">{{ movie.Title }}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app-search',
  data: function () {
    return {
      userInput: '',
      movies: []
    }
  },
  methods: {
    async titleSearch () {
      this.movies = []
      const response = await fetch(`https://www.omdbapi.com/?apikey=5044463d&s=${this.userInput}`)
      const movies = await response.json()
      return movies.Response ? this.movies = movies.Search : false
    },
    getMovieDetails (e) {
      this.userInput = e.target.innerText
      this.movies = []
      return this.$emit('find',e.target.innerText)
    }
  }
}
</script>

<style scoped lang="scss">
.search_wrap {
  background-color: #87BCDE;
  position: relative;
  padding-top: 59px;
  .input_wrap {
    width: 300px;
    margin: 0 auto;
    position: absolute;
    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    .input_field {
      border: .1em solid #243B4A;
      padding: .5em;
      font-size: 1em;
      display: block;
      background-color: #fff;
      text-align: left;
      width: 100%;
      box-sizing: border-box;
      color: #243B4A;
    }
    .input_btn {
      @extend .input_field;
      border-top: none;
      border-bottom: none;
      width: 100%;
      background: #fff;
      &:last-of-type {
        border-bottom: .1em solid #000;
      }
      &:hover {
        background-color: #d2d2d2;
        transition: background-color .1s ease-out;
        cursor: pointer;
      }
    }
  }
}

</style>
