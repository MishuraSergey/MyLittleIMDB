<template>
  <div>
    <div class="content_wrap">
      <app-search @find="getMovieDetails"></app-search>
      <app-result :movie="movie"></app-result>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app-homepage',
  data: function () {
    return {
      movie: null,
    }
  },
  methods: {
    async getMovieDetails (movie) {
      this.movie = ''
      const response = await fetch(`https://www.omdbapi.com/?apikey=5044463d&t=${movie}`)
      const movies = await response.json()
      return movies.Error ? false : this.movie = movies
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');

  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
  }
  .content_wrap {
    max-width: 1920px;
    margin: 0 auto;
    font-size: 16px;
  }
</style>