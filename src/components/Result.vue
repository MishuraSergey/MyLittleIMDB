<template>
  <div v-if="movie">
    <div class="result_wrap">
      <img class="result_poster" v-bind:src="movie.Poster" alt="">
      <p class="result_field result_title">{{movie.Title}}</p>
      <p class="result_field"><b>Year:</b> {{movie.Year}}</p>
      <p class="result_field"><b>Genre:</b> {{movie.Genre}}</p>
      <p class="result_field"><b>Director:</b> {{movie.Director}}</p>
      <p class="result_field"><b>Cast:</b> {{movie.Actors}}</p>
      <p class="result_field"><b>Plot:</b> {{movie.Plot}}</p>
      <p class="result_field"><b>Country:</b> {{movie.Country}}</p>
      <p class="result_field result_ratings">
        <span class="result_ratings__rating" v-for="item in movie.Ratings" :key="item.Source"><b>{{ item.Source }}</b>:
          {{ item.Value }}</span>
      </p>
      <iframe class="result_trailer" :src="trailerId" allowfullscreen></iframe>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app-result',
  props: {
    movie: {
      type: Object,
      required: true
    }
  },
  data: function () {
    return {
      trailerId: '',
      defaultURL: 'https://www.googleapis.com/youtube/v3/search?part=snippet&key=AIzaSyDo57m0oDyT7FUw8ztoO4UoKsg3rvNGoxE&type=video&videoDefinition=high'
    }
  },
  watch: {
    movie: async function(movie) {
      if (movie){
          const response = await
          fetch(`${this.defaultURL}&q=${this.movie.Title}+${this.movie.Year}+trailer`);
          const trailer = await response.json()
          this.trailerId = 'https://www.youtube.com/embed/'+trailer.items[0].id.videoId
      }
    }
  }
}
</script>

<style scoped lang="scss">
.result_wrap {
  margin: 0 auto;
  width: 100%;
  max-width: 768px;
  padding: 10px;
  .result_poster {
    display: block;
    border: 0;
    font-size: 0;
    width: 100%;
    max-width: 300px;
    margin: 0 auto 10px;
  }
  .result_field {
    margin-bottom: 10px;
  }
  .result_title {
    font-size: 1.5em;
    font-weight: bold;
  }
  .result_ratings {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    .result_ratings__rating {
      &:not(:last-of-type){
        margin-right: 10px;
      }
    }
  }
  .result_trailer {
    width: 100%;
    height: 52vw;
    max-height: 428px;
    border: 0;
  }
}
</style>
